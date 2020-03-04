---
title: 使用 docker 搭建 Hadoop 集群
date: 2020-03-04 10:44:56
updated: 2020-03-04 10:44:56
tags:
- hadoop
- pi
- docker
---
　　由于疫情原因手上只有一块树莓派，不能使用虚拟机搭建 Hadoop 集群，docker 上已有的镜像也不支持 ARM64 架构。因此就用 docker 手动配置了 Hadoop 集群，记录一下。由于对 docker 和 hadoop 了解还不够深入，手法可能很丑。
<!-- more -->

### 安装 docker
　　树莓派上使用的是 arch 系的 manjaro，其它系统类似。
```bash
sudo pacman -S install docker docker-compose docker-machine
sudo usermod -a -G docker $user
sudo systemctl start docker
```

### 创建容器
```bash
docker pull alpine
docker pull alpine
docker network create --driver=bridge hadoop
docker run -itd --name=master --net=hadoop alpine
```

### 安装 hadoop
```bash
docker attach master
apk update
apk add openjdk8 openssh bash ncurses shadow openrc
cd /var
wget http://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
tar xzvf hadoop-3.2.1.tar.gz
mv hadoop-3.2.1 hadoop
rm hadoop-3.2.1.tar.gz
```

### 配置环境变量
```bash
# 修改 /etc/profile
vi /etc/profile
# 添加环境变量
export PATH=/var/hadoop/bin
export JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk
export HADOOP_HOME=/var/hadoop/
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-DJava.library.path=$HADOOP_HOME/lib"
export JAVA_LIBRARY_PATH=$HADOOP_HOME/lib/native:$JAVA_LIBRARY_PATH
```
```bash
# 生效环境变量
source /etc/profile
# 启动 sshd
rc-update add sshd
touch /run/openrc/softlevel
/etc/init.d/sshd start
```

### 配置用户组与密钥
```bash
adduser hadoop
usermod -aG hadoop hadoop
chown hadoop:root -R /var/hadoop
chmod g+rwx -R /var/hadoop
su - hadoop
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
```
### 配置 hadoop
#### 配置 hadoop-env.sh
```bash
# 修改 hadoop-env.sh
vi /var/hadoop/etc/hadoop/hadoop-env.sh 
# 修改 JAVA_HOME
export JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk
```

#### 配置 core-site.xml
```bash
vi /var/hadoop/etc/hadoop/core-site.xml
```
```xml
<!-- <conﬁguration> 标签内添加 -->
<property>
    <name>fs.default.name</name>
    <value>hdfs://localhost:9000</value>
</property>
```

#### 配置 yarn-site.xml
```bash
vi /var/hadoop/etc/hadoop/yarn-site.xml
```
```xml
<!-- <conﬁguration> 标签内添加 -->
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
<property>
    <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
```

#### 配置 mapred-site.xml
```bash
cp /var/hadoop/etc/hadoop/mapred-site.xml.template /var/hadoop/etc/hadoop/mapred-site.xml
vi /var/hadoop/etc/hadoop/mapred-site.xml
```
```xml
<!-- <conﬁguration> 标签内添加 -->
<property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
</property>
```

#### 配置 hdfs-site.xml
```bash
vi /var/hadoop/etc/hadoop/hdfs-site.xml
```
```xml
<!-- <conﬁguration> 标签内添加 -->
<property>
    <name>dfs.replication</name>
    <value>3</value> 
</property>
<property>
    <name>dfs.namenode.name.dir</name>
    <value> file:/var/hadoop/hadoop_data/hdfs/namenode</value>
</property>
<property>
    <name>dfs.datanode.data.dir</name>
    <value> file:/var/hadoop/hadoop_data/hdfs/datanode</value>
</property>
```

### 创建 DataNode
```bash
mkdir -p /var/hadoop/hadoop_data/hdfs/datanode
CRTL P+Q
```

### 创建 hadoop 模板镜像
```bash
docker ps -a
docker commit master hadoop-raw
```

### 创建配置 data1 容器
```bash
docker run -itd --name=data1 --net=hadoop hadoop-raw
docker attach data1
```
#### 修改 core-site.xml
```bash
vi /var/hadoop/etc/hadoop/core-site.xml
```
```xml
<!-- 修改原值 -->
<value>hdfs://master:9000</value>
```

#### 修改 yarn-site.xml
```bash
vi /var/hadoop/etc/hadoop/yarn-site.xml
```
```xml
<!-- <conﬁguration> 标签内添加 -->
<property>
    <property>
        <name>yarn.resourcemanager.resource-tracker.address</name>
        <value>master:8025</value>
    </property>
    <property>
        <name>yarn.resourcemanager.scheduler.address</name>
        <value>master:8030</value>
    </property>
    <property>
        <name>yarn.resourcemanager.address</name>
        <value>master:8050</value>
    </property>
</property>
```

#### 修改 mapred-site.xml
```bash
vi /var/hadoop/etc/hadoop/mapred-site.xml
```
```xml
<!-- <conﬁguration> 标签内添加 -->
<property>
    <name>mapred.job.tracker</name>
    <value>master:54311</value>
</property>
```

#### 修改 hdfs-site.xml
```bash
vi /var/hadoop/etc/hadoop/hdfs-site.xml
# 删除namenode所在的<property>标签
```

### 更新 hadoop 模板镜像，创建 data2 3 容器
```bash
docker commit data hadoop-raw
docker run -itd --name=data2 --net=hadoop hadoop-raw
docker run -itd --name=data3 --net=hadoop hadoop-raw
```

### 创建 NameNode
```bash
docker attach master
mkdir -p /var/hadoop/hadoop_data/hdfs/namenode
hadoop namenode -format
```

### 启动 hadoop 集群
```bash
start-all.sh
```
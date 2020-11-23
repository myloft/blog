---
title: OpenWrt ZeroTier 跨地区组网
date: 2020-11-23 12:00:00
updated: 2020-11-23 12:00:00
tags: Network
---
　　之前一直在用 Ocserv 或者 Softether 进行组网，但是这两者不能比较好的去中心化，且需要公网 IP，总的来说不适合跨地区的家庭组网。这次考虑使用 ZeroTier 进行组网，参考了相关的教程，发现很多都选择采用 iptables 对网络进行 NAT 的方式，不仅配置繁琐，还会损失性能。这里直接使用静态路由的方式，无需多余配置，性能也比较好。

<!-- more --> 
　　以一个三节点的网络为例：两台路由器 Openwrt 版本分别为 18.06 与 19.04，一台服务器部署了 Ocserv 用于不能直接使用 ZeroTier 设备通过 VPN 接入，网段如下。
```
ZeroTier：10.244.0.0/24
路由器 A（Openwrt）：172.16.0.1/16
路由器 B（Openwrt）：172.17.0.1/16
服务器 C（Ocserv）：192.168.30.0/24
```
　　关于 ZeroTier 注册和 Ocserv 相关的设置就不赘述了，主要关注 Openwrt 的相关的配置。

## 安装 ZeroTier
　　安装 ZeroTier 非常简单，使用 ssh 登录路由器，使用包管理器安装即可，使用 LUCI 图形化安装也行，不过下面的配置仍需要在命令行中进行。
```sh
$ opkg update
$ opkg install zerotier
```

## 配置 ZeroTier
　　安装完成后，编辑配置文件 `vi /etc/config/zerotier`，初始配置文件如下：
```
config zerotier sample_config
        option enabled 0

        # persistent configuration folder (for ZT controller mode)
        #option config_path '/etc/zerotier'

        #option port '9993'

        # Generate secret on first start
        option secret 'generate'

        # Join a public network called Earth
        list join '8056c2e21c000001'
```
　　需要修改的地方只有两处：
1. `option enabled 0` 将 `0` 修改为 `1`。
2. `list join '8056c2e21c000001'` 将 Earth 网络 id 修改自己的私有网络 id。

　　完成后执行命令启动即可。
```sh
$ /etc/init.d/zerotier start
```
　　可以通过命令查看连接到的网络，建议不用着急去 ZeroTier 网页配置 IP，等全部配置完成后再进行统一规划，避免敲错网段导致断网（逃。
```sh
$ zerotier-cli listnetworks
```

## 创建接口与防火墙
　　为了便于配置，我们需要为 ZeroTier 创建的网卡创建一个接口和独立的防火墙，如图所示：
　　创建一个叫 ZeroTier 的接口，绑定 ZeroTier 创建的虚拟网卡。
![创建接口](/images/zerotier-create-interface.png)
　　保存后，为 ZeroTier 创建一个专属的防火墙区域，也叫 ZeroTier。
![创建防火墙](/images/zerotier-create-firewall.png)

## 配置防火墙
　　创建完成后，配置 ZeroTier 的防火墙，如图所示：
1. Input、Output、Forward 全部允许。
2. 允许 ZeroTier 转发至 lan。
3. 允许 ZeroTier 转发自 lan。
![配置防火墙](/images/zerotier-firewall.png)

## Zerotier 网页配置
　　接着登录 ZeroTier 网站，为我们的设备分配 IP。
```
ZeroTier：10.244.0.0/24
路由器 A（Openwrt）：172.16.0.1/16      10.244.16.1
路由器 B（Openwrt）：172.17.0.1/16      10.244.17.1
服务器 C（Ocserv）：192.168.30.0/24     10.244.30.1
```
　　接着在网页添加静态路由，如图所示：
![静态路由](/images/zerotier-route.png)

## 测试
　　完成上面的配置后，网络就打通了，无需使用 iptables 进行 NAT。
　　在路由器 B 上执行 `ip route` 可以看到 ZeroTier 下发的路由表。
```sh
$ ip route
10.244.0.0/16 dev ztyouvscim proto kernel scope link src 10.244.17.1
172.16.0.0/16 via 10.244.16.1 dev ztyouvscim
172.17.0.0/16 dev br-lan proto kernel scope link src 172.17.0.1
192.168.30.0/24 via 10.244.30.1 dev ztyouvscim
```
　　在路由器 A 上也可以 traceroute B 路由器下的设备，说明我们完成了组网的工作。
![路由追踪](/images/zerotier-traceroute.png)

## 参考链接
　　如果对配置看得不是很懂，可以参考下面这篇文章，配图更加详细，防火墙的配置按照本文来即可。
1. [Openwrt路由通过Zerotier组网实现异地内网互访](https://engrzhou.github.io/2018/07/Openwrt%E8%B7%AF%E7%94%B1%E9%80%9A%E8%BF%87Zerotier%E7%BB%84%E7%BD%91%E5%AE%9E%E7%8E%B0%E5%BC%82%E5%9C%B0%E5%86%85%E7%BD%91%E4%BA%92%E8%AE%BF/)
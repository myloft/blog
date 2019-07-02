---
title: Caddy + PHP简易教程
date: 2018-11-02 18:38:51
updated: 2018-11-02 18:38:51
tags: Linux
toc: true
---

　　最近入坑PT，入了Online的独服刷流量。各大教程中Rutorrent都是配合Nginx的，有点麻烦。平时Caddy用的比较多就换成了Caddy。使用Caddy的话可以更加方便的开启TLS和BasicAuth。
<!-- more --> 

安装caddy
---
```
curl https://getcaddy.com | bash -s personal
```

安装PHP Tmux
---
```
apt install php7.2-fpm tmux
```

配置PHP
---
```
nano /etc/php/7.2/fpm/pool.d/www.conf
```
```
#修改运行用户和运行Caddy的用户一致
user = iloft
group = iloft
#修改监听
listen = 127.0.0.1:9000
#修改网站目录位置
;chroot = /var/www
;chdir = /var/www
```

配置Caddy
---
```
cd /var
mkdir www
cd ~
nano Caddyfile
```
```
#配置文件
blog.iloft.xyz {
    root /var/www
    fastcgi / 127.0.0.1:9000 php
}
```

运行服务
---
```
#重启PHP-FPM
systemctl restart php7.2-fpm
#运行Caddy
tmux
caddy -conf Caddyfile
#输入邮箱自动签发证书
```

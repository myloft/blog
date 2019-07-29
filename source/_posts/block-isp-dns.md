---
title: iptables解决运营商DNS抢答(我劫持我自己)
date: 2019-01-23 21:06:45
updated: 2019-01-23 21:06:45
tags: Network
---
　　春节带着NAS回家，发现UT里的种子一片红。本以为是CloudFlare连通性不好，但PT主站都能正常访问。尝试ping发现tracker服务器被解析到了127.0.0.1。  
<!-- more --> 

检测
---
```
//响应127.0.0.1确认DNS被劫持。
nslookup whether.114dns.com 114.114.114.114
```
　　确认到dns被劫持，接下来就需要找到抢答的服务器IP。直接从[网易DNS检测工具](http://nstool.netease.com/)看到目前使用的DNS服务器是运营商提供的153.3.231.218。（不排除某些情况下需要抓包)  

解决
---
  ~~直接在openwrt防火墙自定义规则里将该ip段的流量全部drop掉。~~
```
iptables -I INPUT -s 153.3.231.0/16 -j DROP //请无视
```
结果
---
```
//响应58.217.249.139确认成功使用114dns解析。
nslookup whether.114dns.com 114.114.114.114
```
　　更新一下tracker红种全部变蓝，可以继续愉快做种了~~~

打脸
---
　　早上尝试抓包，然后再水一篇**wireshark抓包定位DNS抢答服务器**。然而并没有出现抢答现象，响应ip就是查询的NS服务器，说明昨天一顿操作......
![伪造的dns响应](/images/fake-dns-response.jpg)  
　　仔细看了下iptables发现....是我劫持了我自己
```
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 53
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 53
```
　　所有的dns查询都被转发给了路由器，路由器从运营商dns拿到解析，然后还贴心的伪造了响应。我的天（说明网上的固件不要乱刷...鬼知道里面还塞了什么）！
　　删掉这条之后，dns解析就正常了。
![伪造的dns响应](/images/real-dns-response.jpg)  
　　天知道我昨天是怎么解析出来正确ip的，给联通道个歉~
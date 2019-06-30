---
title: OpenWrt同网段桥接配置
date: 2019-06-30 12:00:00
tags: 网络
---
　　最近组了个ITX主机，丐版主板不带无线网卡，坐的地方也没网口。因此尝试主机用网线连接从路由器，路由器之间通过WIFI桥接。
<!-- more --> 
　　主路由器为K3，从路由器为K2P同配置的友华1200js。家里的网段为172.16.0.0/16，主路由器网关为172.16.0.1。尝试了两种方法来实现两路由器下设备在同一网段。

优雅的方法（LAN Bridge）
---
　　比较优雅的方法是将从路由设置的IP设置在同一网段，如172.16.0.2，并关闭DHCP。按正常方式扫描WIFI并加入网络，再将接口从WWAN修改为LAN，最后删除自动创建的WWAN接口即可完成配置。
![修改Network](/images/wwan-2-lan.jpg) 
　　这种直接桥接的方式最为优雅，主路由器、从路由器、各路由器下的设备都在同一网段，无需路由器转发通讯。然而实际配置时出现了无线无法启动的问题。
![无线无法启动](/images/wireless-not-associated.jpg) 
　　经过搜索发现MT7615的驱动并不支持Wireless与Lan桥接模式。但在另一台路由器EA6350上成功实现。
![系统日志](/images/bridge-not-allowed.jpg) 

通用方法（Relay）
---
　　通用的方法是对LAN与WWAN进行转发，效率没有直接桥接高。

#### 加入网络
　　这种方式从路由器与主路由器不在同一网段，如192.168.1.1，关闭DHCP。按正常方式扫描WIFI并加入网络。此时可以通过设置PC的网关为192.168.1.1正常上网。
![加入网络](/images/join-wireless.jpg)

#### 安装软件包
　　为了实现转发，需要安装relayd和luci-proto-relay两个包，如果是一些定制固件可能已经包含了。
```
opkg update
opkg install luci-proto-relay
```

#### 创建转发
　　在网络接口点击创建新的网络接口，如命名为RELAY，选择接口类型为Relay bridge。
![创建网络接口](/images/create-interface.jpg)
　　配置上一步创建的RELAY接口，将lan和wwan加入转发。
![配置网络接口](/images/relay-configuration.jpg)

#### 配置防火墙
　　完成转发配置之后需要配置防火墙，将Forward修改为为accept。
![配置防火墙](/images/firewall-configuration.jpg)

#### 测试网络
　　此时从路由器下的设备就可以通过DHCP获得正确的IP地址和网关了，可以正常联网并与局域网内的设备进行通信了。由于采用了Relay方式，路由追踪略微有些美中不足。
![测试网络](/images/test-network.jpg)
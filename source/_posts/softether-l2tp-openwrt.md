---
title: Openwrt使用Softether配置L2tp
date: 2019-03-22 12:00:00
tags: 网络
---
　　由于主流VPN协议被重点关照，所以之前在服务器上一直使用的Ocserv来配置Cisco的Anyconnect做游戏加速。沿用到路由器上发现并不好用，配置文件比较复杂，手机电脑还要装应用。所以尝试了一下弃用已久的Softether发现挺好用的。

<!-- more --> 

配置环境
---
1. Phicomm K3 Openwrt-18.06
2. Softethervpn(直接在软件包里搜索安装还是opkg安装，甚至自己编译，默认你装好了)
3. vpnsmgr(连接工具 官网下)

Softether配置
---
　　打开SoftEther VPN Server管理器，新建连接配置。填好设置名，以及路由器IP确定并连接。  

![新的连接设置](/images/new-softether.jpg)  

　　第一次设置一下密码，勾选远程访问VPN Server下一步，确定初始化，虚拟HUB名默认就好。  

![新的连接设置](/images/remote-access.jpg)  

　　直接退出动态DNS功能，Openwrt上已经配置了DDNS。勾选启用L2TP服务器功能（L2tp over IPsec），设置IPsec预共享密钥。确认并禁用VPN Azure。  

![开启L2tp](/images/l2tp.jpg)  

![禁用vpn azure](/images/vpn-azure.jpg)  

　　关闭，到主界面，选择管理虚拟HUB，管理用户，新建用户。

![新建用户](/images/new-user.jpg)  

　　关闭，到主界面，选择本地网桥设置，选择之前创建的虚拟HUB，创建新tap设备的桥接，取个名字，创建本地桥。

![创建本地网桥](/images/new-tap.jpg)

　　关闭，点击本地网桥，看到虚拟HUB和tap运行成功。Softether的配置就结束了。  

Openwrt配置
---
　　登录Openwrt点击网络-接口-编辑LAN-物理设置-接口，找到创建的tap并勾上，保存并应用。  

![openwrt-lan](/images/openwrt-lan.jpg)  

　　点击网络-防火墙-流量规则，打开路由器端口，添加L2tp连接所需的端口。  

| 名称  | 协议 | 外部端口 |
| ----- | ---- | -------- |
| L2TP  | TCP + UDP  | 1701     |
| IKE   | UDP  | 500      |
| IPsec | UDP  | 4500     |

![防火墙规则](/images/firewall-ruler.jpg)  

　　到此为止，Softether的L2tp的配置就完成了，连接VPN的设备会和局域网内的设备在同一网段内，可以自由访问。  

总结
---
　　路由器上用Softether VPN配置L2tp相较Ocserv基本是无脑下一步，几乎所有操作系统都原生支持连接，比较适合部署在路由器这种流量不会被干扰的设备上。
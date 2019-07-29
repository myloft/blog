---
title: 修改Windows TTL值解除移动热点限速
date: 2018-04-26 17:36:14
updated: 2018-04-26 17:36:14
tags: Network
---

　　前几天移动手机套餐升级到了无限流量，爽了好几天，然而用笔记本连接手机热点时发现速度极慢，然而手机直接测试网速正常。于是怀疑移动采用了一些技术手段限制用户开启个人热点。

![TTL64](/images/TTL64.png)

<!-- more --> 

　　根据推测可行的方法应该包括TTL检测和MAC检测，尝试将Windows的TTL修改成65。

```
//修改TTL为65
netsh int ipv4 set glob defaultcurhoplimit=65
netsh int ipv6 set glob defaultcurhoplimit=65
```

　　修改完成速度恢复正常，通过Ping测试TTL可能看不出变化，实际上已经修改成功。

![TTL65](/images/TTL65.png)

　　然而未越狱的IOS设备无法修改TTL，因此可以备一个旅行路由器，通过路由器桥接使得TTL-2也可绕过检测。

![wr802n](/images/wr802n.png)
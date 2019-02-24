---
title: 修改Windows网络名
date: 2018-05-09 09:41:02
tags: 网络
---

　　最近一直在玩软路由，配置完后发现windows的网络号已经飙到网络10了。所以需要删除无效的网络配置文件，并自定义一下网络名。

<!-- more --> 

```
//修改注册表
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles
```

![修改注册表](/images/network_profile.png)

Done！

![网络列表](/images/network.png)
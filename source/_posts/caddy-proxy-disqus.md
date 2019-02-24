---
title: 使用caddy反代Disqus API服务器
date: 2018-12-27 11:51:34
tags:
---
　　DisqusJS提供的默认服务器不保证SLA，如果有国外服务器可以自行反代。这种简单反代任务交给Caddy还是很舒心的~~
<!-- more --> 

安装caddy
---
``` shell
curl https://getcaddy.com | bash -s personal
```

安装Tmux
---
``` shell
apt install tmux
```

创建配置文件
---
``` shell
//Caddyfile
comments.abc.xyz {
        proxy /api https://disqus.com/
}
```

运行caddy
---
``` shell
tmux
caddy -conf Caddyfile
```

设置api服务器
---
``` js
//comment.jade
            api: 'comments.abc.xyz', //填写反代服务器地址
```
水完收工~~~
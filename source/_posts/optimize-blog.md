---
title: Brotli压缩 秒开就完事了
date: 2018-12-07 17:04:11
tags: hexo
---
　　TLS1.3正式发布了，博客托管在企鹅家的对象存储上。看了下并没有上TLS1.3，倒是多了智能压缩（Brotli）和HTTP Header配置。顺手打开并加上HSTS响应头。

<!-- more --> 

![腾讯云CDN](/images/qcloud_Brotli.png)

![HTTP_header](/images/header.png)

结论：没啥感觉，反正都是秒开 ~~只是为了水了一篇~~。
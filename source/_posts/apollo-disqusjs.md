---
title: DisqusJS适配Apollo主题
date: 2018-12-27 11:01:26
tags: hexo
---
　　多说倒闭之后，Disqus应该算是是最好的评论系统了，但是Disqus在国内处于半封锁的状态，网站的部分用户可能无法正常访问。Qi在上篇文章的评论指出了这个问题，因此决定使用DisqusJS解决这个问题。先上张效果图，因为我知道你们都能正常访问~~

<!-- more --> 

![disqusjs](/images/disqusjs.png)

　　阁楼使用的Apollo主题的原作者已经停止更新了，没有人适配DisqusJS。看了下主题代码结构还是很清晰的。照着DisqusJS文档简单修改一下就行了。

``` js
//comment.jade
if theme.disqus
    #disqus_thread
    link(href='https://unpkg.com/disqusjs@1.0/dist/disqusjs.css', rel='stylesheet')
    script(src="https://unpkg.com/disqusjs@1.0/dist/disqus.js")
    script.
        var shortname = '#{theme.disqus}';
        var identifier = '#{page.title}';
        var url = '#{config.url}/#{page.path}';
        var apikey = ''; //自行申请Disqus API
        (function() {
            var dsqjs = new DisqusJS({
                shortname: shortname,
                siteName: '',
                identifier: identifier,
                url: url,
                api: '', //留空使用DisqusJS提供的API 也可以自行反代
                apikey: apikey,
                admin: '',
                adminLabel: ''
            });
        })();
```
　　下一篇水一下用Caddy反代Disqus API服务器吧，有能力的可以自行搭建~~~

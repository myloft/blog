---
title: 我也来一个邮件样式~~
date: 2013-7-8 18:19:44
updated: 2013-7-8 18:19:44
tags: 好玩
---
搬家之后~竟然忘记装邮件回复插件！今天做邮件样式的时候才发现~做的很丑啊！拔的是DNSPOD的服务器宕机，今天主机宕机了2分钟。纯属学习~
代码如下：
<!-- more -->    
~~~
<div style="border-bottom: #cccccc 1px solid; border-left: #cccccc 1px solid; padding-bottom: 5px; background-color: #f5f5f5; padding-left: 5px; padding-right: 5px; border-top: #cccccc 1px solid; border-right: #cccccc 1px solid; padding-top: 5px" class="cnblogs_code">     <pre><span style="color: #0000ff">&lt;!</span><span style="color: #ff00ff">DOCTYPE html PUBLIC &quot;-//W3C//DTD XHTML 1.0 Strict//EN&quot; </span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">html</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">head</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">style </span><span style="color: #ff0000">tyle</span><span style="color: #0000ff">=&quot;text/css&quot;</span><span style="color: #0000ff">&gt;</span><span style="color: #000000">
p{}
</span><span style="color: #0000ff">&lt;/</span><span style="color: #800000">style</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">head</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">style</span><span style="color: #0000ff">=&quot;border-bottom:3px solid #d9d9d9;width:900px;&quot;</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">style</span><span style="color: #0000ff">=&quot;border:1px solid #c8cfda;padding:40px;font-size:14px;&quot;</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;!</span><span style="color: #ff00ff">doctype html</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">html</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">head</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">meta </span><span style="color: #ff0000">charset</span><span style="color: #0000ff">=&quot;utf-8&quot;</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">title</span><span style="color: #0000ff">&gt;</span>DNSPod<span style="color: #0000ff">&lt;/</span><span style="color: #800000">title</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">style </span><span style="color: #ff0000">type</span><span style="color: #0000ff">=&quot;text/css&quot;</span><span style="color: #0000ff">&gt;</span><span style="color: #000000">
body {
    font-size: 14px;
    color: #5e5e5e;
    line-height: 22px;
}
#d-main p {
    padding: 20px 30px;
    font-size: 14px;
    margin: 0;
}
#d-main ul {
    padding: 20px 30px;
    font-size: 14px;
    margin: 0;
    margin-left: 10px;
}
#d-main a {
    color: #1A6CC1;
    text-decoration: none;
}
#d-main {
    width: 800px;
    margin: 15px auto;
}
#d-main-box {
    border: 1px solid #CBD9DB;
}
#d-main-header {
    height: 60px;
    background: #2da1da;
    color: #fff;
}
#d-main-header h1 {
    font-size: 32px;
    font-weight: bold;
    padding-left: 30px;
    padding-top: 20px;
    margin: 0;
}
#d-main-footer {
    border-top: 1px solid #d7e3e5;
}
#d-main-footer p {
    text-align: center;
    padding: 15px;
    padding-bottom: 5px;
}
.d-main-line {
    background: #ebf5f6;
    border-top: 1px solid #d7e3e5;
    padding: 10px 15px;
    font-weight: bold;
}
</span><span style="color: #0000ff">&lt;/</span><span style="color: #800000">style</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">head</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">body</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">id</span><span style="color: #0000ff">=&quot;d-main&quot;</span><span style="color: #0000ff">&gt;</span>
    <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">id</span><span style="color: #0000ff">=&quot;d-main-box&quot;</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">id</span><span style="color: #0000ff">=&quot;d-main-header&quot;</span><span style="color: #0000ff">&gt;</span>
            <span style="color: #0000ff">&lt;</span><span style="color: #800000">h1</span><span style="color: #0000ff">&gt;</span>您在[blogname]有新回复啦<span style="color: #0000ff">&lt;/</span><span style="color: #800000">h1</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">class</span><span style="color: #0000ff">=&quot;d-main-content&quot;</span><span style="color: #0000ff">&gt;</span>
            <span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>[pc_author], 您好!<span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
            <span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>您于[pc_date] 在文章《[postname]》上发表评论:<span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
      <span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">class</span><span style="color: #0000ff">=&quot;d-main-line&quot;</span><span style="color: #0000ff">&gt;</span><span style="color: #000000">
            评论内容</span><span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">class</span><span style="color: #0000ff">=&quot;d-main-content&quot;</span><span style="color: #0000ff">&gt;</span>
            <span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>[pc_content]<span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">class</span><span style="color: #0000ff">=&quot;d-main-line&quot;</span><span style="color: #0000ff">&gt;</span><span style="color: #000000">
            [cc_author]于[cc_date] 给您的回复如下:
        </span><span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">class</span><span style="color: #0000ff">=&quot;d-main-content&quot;</span><span style="color: #0000ff">&gt;</span>
            <span style="color: #0000ff">&lt;</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>[cc_content]<span style="color: #0000ff">&lt;/</span><span style="color: #800000">p</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">class</span><span style="color: #0000ff">=&quot;d-main-line&quot;</span><span style="color: #0000ff">&gt;</span><span style="color: #000000">
            想了解更多吗？</span><span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">class</span><span style="color: #0000ff">=&quot;d-main-content&quot;</span><span style="color: #0000ff">&gt;</span>
            <span style="color: #0000ff">&lt;</span><span style="color: #800000">ul</span><span style="color: #0000ff">&gt;</span>
                <span style="color: #0000ff">&lt;</span><span style="color: #800000">li</span><span style="color: #0000ff">&gt;</span>您可以点击 <span style="color: #0000ff">&lt;</span><span style="color: #800000">a </span><span style="color: #ff0000">href</span><span style="color: #0000ff">=&quot;[commentlink]&quot;</span><span style="color: #ff0000"> target</span><span style="color: #0000ff">=&quot;_blank&quot;</span><span style="color: #0000ff">&gt;</span>查看回复的完整內容<span style="color: #0000ff">&lt;/</span><span style="color: #800000">a</span><span style="color: #0000ff">&gt;&lt;/</span><span style="color: #800000">li</span><span style="color: #0000ff">&gt;</span>
                <span style="color: #0000ff">&lt;</span><span style="color: #800000">li</span><span style="color: #0000ff">&gt;</span>感谢你对 <span style="color: #0000ff">&lt;</span><span style="color: #800000">a </span><span style="color: #ff0000">href</span><span style="color: #0000ff">=&quot;http://www.freehao123.com/&quot;</span><span style="color: #ff0000"> target</span><span style="color: #0000ff">=&quot;_blank&quot;</span><span style="color: #0000ff">&gt;</span>可可豆的博客<span style="color: #0000ff">&lt;/</span><span style="color: #800000">a</span><span style="color: #0000ff">&gt;</span> 的关注，如您有任何疑问，欢迎在博客留言，我会一一解答<span style="color: #0000ff">&lt;/</span><span style="color: #800000">li</span><span style="color: #0000ff">&gt;</span>
            <span style="color: #0000ff">&lt;/</span><span style="color: #800000">ul</span><span style="color: #0000ff">&gt;</span>
      <span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">class</span><span style="color: #0000ff">=&quot;d-main-line&quot;</span><span style="color: #0000ff">&gt;&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
      <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">class</span><span style="color: #0000ff">=&quot;d-main-content&quot;</span><span style="color: #0000ff">&gt;&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
        <span style="color: #0000ff">&lt;</span><span style="color: #800000">div </span><span style="color: #ff0000">id</span><span style="color: #0000ff">=&quot;d-main-footer&quot;</span><span style="color: #0000ff">&gt;</span>
            <span style="color: #008000">&lt;!--</span><span style="color: #008000">&lt;p&gt;&lt;/p&gt;</span><span style="color: #008000">--&gt;</span>
        <span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
    <span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">div</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">body</span><span style="color: #0000ff">&gt;</span>
<span style="color: #0000ff">&lt;/</span><span style="color: #800000">html</span><span style="color: #0000ff">&gt;</span></pre>
  </div></p>
  ~~~
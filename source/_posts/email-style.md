---
title: 我也来一个邮件样式~~
date: 2013-7-8 18:19:44
updated: 2019-7-5 16:30:00
tags: 好玩
---
　　搬家之后~竟然忘记装邮件回复插件！今天做邮件样式的时候才发现~做的很丑啊！拔的是DNSPOD的服务器宕机，今天主机宕机了2分钟。纯属学习~
<!-- more -->
<div class="tip">
    　　6年后的2019年7月5日配置Valine-admin邮件模版时才发现当时发布的模版是根本没法使用的。并且发现实在是太丑了，因此尽管修复了代码并进行了适配，并不准备使用该模版\~\~\~
</div>
代码如下：   
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" >
<html>
<head>
<meta charset="utf-8">
<style type="text/css">
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
</style>
</head>
<body>
<div id="d-main">
    <div id="d-main-box">
        <div id="d-main-header">
            <h1>您在${SITE_NAME}上有新的回复</h1>
        </div>
        <div class="d-main-content">
            <p>${PARENT_NICK}, 您好!</p>
      </div>
        <div class="d-main-line">
            您曾在${SITE_NAME}上发表评论:</div>
        <div class="d-main-content">
            <p>${PARENT_COMMENT}</p>
        </div>
        <div class="d-main-line">
            ${NICK}给您的回复如下:
        </div>
        <div class="d-main-content">
            <p>${COMMENT}</p>
        </div>
        <div class="d-main-line">
            想了解更多吗？</div>
        <div class="d-main-content">
            <ul>
                <li>您可以点击 <a href="${POST_URL}" target="_blank">查看回复的完整內容</a></li>
                <li>感谢你对 <a href="${SITE_URL}" target="_blank">${SITE_NAME}</a> 的关注，如您有任何疑问，欢迎在${SITE_NAME}留言，我会一一解答</li>
            </ul>
      </div>
        <div class="d-main-line"></div>
      <div class="d-main-content"></div>
        <div id="d-main-footer">
            <!--<p></p>-->
        </div>
    </div>
</div>
</body>
</html>
```
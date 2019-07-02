---
title: Linux知识：为什么要用字符~来表示home目录
date: 2013-6-28 19:47:06
updated: 2013-6-28 19:47:06
tags: Linux
---
在Unix风格的操作系统里(包括BSD, GNU/Linux 和 Mac OS X)，通常用波浪号“~”来表示当前用户的主目录(home目录)：例如，如果当前用户的home目录是/home/bloggsj，那么，输入 cd 或 cd ~ 或 cd /home/bloggsj 或 cd $HOME 都是等效的。这种习惯源自于1970年代流行的Lear-Siegler ADM-3A终端机，这种机器上波浪号和“home”键(用于把光标移动到最左端)正好在同一个键上。
<!-- more --> 
![Exchange](http://www.vaikan.com/wordpress/wp-content/uploads/2013/06/Exchange.png)

下面是Lear-Siegler ADM-3A终端机的一些照片：

![Lear Siegler - ADM3A Terminal (ca. 1976)](http://www.vaikan.com/wordpress/wp-content/uploads/2013/06/Lear_Siegler-ADM3A_1782.jpg)

Lear Siegler – ADM3A Terminal (ca. 1976)

![L3esv](http://www.vaikan.com/wordpress/wp-content/uploads/2013/06/L3esv-800x350.jpg)

[英文原文：[Why was '~' chosen to represent the home directory?](http://unix.stackexchange.com/questions/34196/design-question-why-was-chosen-to-represent-the-home-directory) ]
---
layout: post
title: "mac设置本地web服务"
description: ""
category: 其他
tags: [其他]
---
{% include JB/setup %}

####1.在mac本地创建web站点

**系统偏好设置->共享->web共享**,可以看到个人网站的站点目录和我的电脑的站点目录，点击其中的url地址，在url可以打开。点击电脑站点目录，可以进入到站点文件夹，可以将web开发资源放于此处，使用浏览器即能打开。

**个人站点本地目录：**/Users/username/Sites/  
**电脑站点目录：**/Library/WebServer/Documents

####2.测试是否工作
浏览器测试输入 localhost，若正常工作则可以看到显示  
**It works！**  

若出现错误，则很可能apache php未配置正确 
####3.配置apache  
若未配置好apache php服务，修改/etc/apache2/httpd.conf 对应配置即可   
**xxx$ open /etc/apache2/httpd.conf -a TextWrangler**  
**xxx$ sudo vim /etc/apache2/httpd.conf**  
####4.mac 10.8以上系统
在mac os 10.8.5中 **“web共享”已经取消**，此时命令行下启动apache 即可。  

 * sudo apachctl start
 * 在blog目录下激动jekyll服务  jekyll serve ，`启动jekyll服务`
 * 在浏览器种输入http://localhost:4000//     
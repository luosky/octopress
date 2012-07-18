---
layout: post
title: "iOS Notification 学习笔记"
date: 2010-10-31 00:17
comments: true
external-url: 
categories: [iOS,Programming]

---

##分类
1. 本地notification 
	
	* 4.0才开始支持
	* 无需网络,无需走和Apple推送服务器的整个推送流程,一切都在本地发生
2. push notification 
	
	* 3.0开始支持
	* 对于因为网络原因未发送到用户手机上的notification ,APNs会保留起来,待该device联网后发送.但对每台机器的每个应用只会保留一条,后来的覆盖之前的.
	* payload大小不超过256byte
	* **每次应用程序启动**时都需向APNs注册,获得device token后将其传回应用的provider(application的server端)
	* provider要发送notification时,需附带这个device token
	* APNs会反馈持续发送失败的device 列表给provider,provider应停止向这些device发送notification
	* icon budge上显示的数字就是provider发过来的数字,不会累加.

##接收到notification时的处理
本地notification和push notification对用户来讲没什么不同,当用户接收到一个notification时:

  * 若应用在后台运行,或者没有运行时
	  * 展现这个notification(提醒,声音和icon badge).若用户点了这个提醒(或是划动解锁了屏幕),则以notification带的payload作为参数调用`application:didFinishLaunchingWithOptions:`方法启动应用
  * 若正在前台运行
	  * 若是push notification,则`application:didReceiveRemoteNotification: `被调用
	  * 若是本地的notification,则 `application:didReceiveLocalNotification: ` 会被调用

##建立推送服务器

要发送notification的provider需先获取SSL证书.一个证书只能用于一个应用.且有测试(sandbox)证书和正式证书之分:

  * 测试(sandbox):[gateway.sandbox.push.apple.com][11], outbound TCP port 2195.
  * 正式:[gateway.push.apple.com][12], outbound TCP port 2195.

   [11]: http://gateway.sandbox.push.apple.com
   [12]: http://gateway.push.apple.com

iOS Develop Program的用户有三种角色:

1. team agent
2. team admin
3. team member.

只有team agent可以创建SSL证书  
team agent 和team admin可以创建provisioning profile  
team member只能下载和安装证书及profile



---
layout: post
title: "使用TestFlight 安装测试版iOS应用(面向测试人员)"
date: 2012-11-30 11:11
comments: true
external-url: 
categories: iOS
---


之前使用的AirTest有不稳定时常装不上和一定要在内网才能安装等缺点,打算换用TestFlight来进行iOS测试版的安装部署.

TestFlight主要有以下几个好处:

1. 随时随地下载发布的测试版应用
2. 新版本通知. 这样以后有新版本发布大家马上就能安装测试,十分方便.
3. 新版本可带有release note, 测试人员看了可以有针对性的进行测试


#### 安装步骤:

1. 到开发人员提供的邀请网址里注册TestFlight帐号. 完成这步你的帐号就会加到组里.
2. 注册完之后会收到一封注册右键,**在你的iOS设备上** 点击邮件里的登录按钮 (iOS设备指iPhone,iPod touch或iPad)
	* 若你的iOS设备没有设置邮箱的话,也可以用safari打开[https://testflightapp.com/m/login](https://testflightapp.com/m/login) 这个链接,然后用你注册的帐号登录
4. 按照登录后的网页提示安装证书.完成这步会将你的设备和帐号对应起来.
5. 若你是在safari中打开邮件里的链接的话,此时你可以将这个网页**[添加到主屏幕](http://www.apple.com.cn/ios/add-to-home-screen/)**. 这会在你的iOS桌面上生成一个TestFlight的应用,以后要下载新版本的应用可以直接打开这个应用下载,不用再输入地址了.
5. 等开发人员为你的设备开通权限后,即可在这台设备上下载测试app了.

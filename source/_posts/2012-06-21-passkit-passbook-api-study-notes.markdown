---
layout: post
title: "PassKit (PassBook api) 学习笔记"
date: 2012-06-21 21:40
comments: true
external-url: 
categories: [programming, iOS]

---

##PassKit 学习笔记

PassBook里存放的是一个个电子凭证(pass),这些pass可以是兑换券,机票,门票等,这些pass可以通过各种第三方app,网站,或者email进行安装. PassBook本身不提供这些pass,只是提供了一个查看这些pass,以及接受pass更新的平台. PassKit则是这个平台开放的API.


###pass的安装方式
1. 通过网站下载 (MIME type: application/vnd-com.apple.pkpass)
2. 通过email下载
3. 通过提供pass的app安装

###pass type identifier

必须在developer.apple.com里注册自己pass type identifier

###pass的类型
* boardingPass
* coupon
* eventTicket
* storeCard  余额卡,必须指定余额值
* generic

###pass的提醒方式

**预先指定的提醒:**

* 时间 --当时间接近时可在锁屏界面提醒用户相关信息
* 地点 --当用户接近指定地点时提醒用户相关信息(指定地点最多只能指定10个)

预先指定的提醒在发布到用户的PassBook后仍可以随时被修改.


**实时提醒:**

* 当pass信息需要更改时,可通过推送通知用户,界面上可展示相应的修改信息. (除了pass的类型和序列号,其他字段都可以随时修改)


### pass的样式

#### 颜色:
![image](/images/blog/2012-06-21-passkit-passbook-api-xue-xi-bi-ji/color.jpg)

##### 样式:

store card & coupon:

![image](/images/blog/2012-06-21-passkit-passbook-api-xue-xi-bi-ji/logo_bg_storecard.png)

event :

![image](/images/blog/2012-06-21-passkit-passbook-api-xue-xi-bi-ji/logo_bg_event.png)

###pass的字段

![image](/images/blog/2012-06-21-passkit-passbook-api-xue-xi-bi-ji/fields.jpeg)

* serialNumber用来标识pass,同样的pass type identifier 下不能有两个pass的serialNumber相同
* 只有发布时指定了验证地址和验证token的pass才能在发布后进行修改(这个token只是用来验证pass本身可以接收更新,和设备是没有关系的)
* webServiceURL token的验证地址,指向用户自己实现的rest服务器,必须实现更新pass所需的各种接口,必须是https…
* relevantDate 提醒用户的时间. 具体显示提醒的时间范围由pass类型决定
* locations 提醒用户的地址.范围大小由pass类型决定
* 显示App Store item : 
`
"associatedStoreIdentifiers" : [ 375380948 ]
`
###注意点


* iPad上没有Passbook  
* 导出P12时不用选中私钥
* 签名和打包pass可用wwdc示例代码提供的签名工具
* BARCODE不支持中文(应该跟用的编码有关)
* 日期格式: 2012-06-20T17:30+08:00
* 使用相对时间的格式  "isRelative" : true
* coupon的primaryFields很大,中文只能放3个字
* PKPassLibrary不是singleton,注册PKPassLibraryDidChangeNotification通知时,通知是针对某个具体的library对象的,所以需将你创建的library传过去.

``` objective-c

    [noteCenter addObserver:self                   selector:@selector(passLibraryDidChange)                       name:PKPassLibraryDidChangeNotification                     object:self.library];
```


附上自己写的 [pass工程](/images/blog/2012-06-21-passkit-passbook-api-xue-xi-bi-ji/passkit_project.zip)  (你可能需要换成自己开发者账号签名的pass才能在pass列表中查看已安装的pass,添加pass的话则不需要)




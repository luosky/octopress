---
layout: post
title: "First Week on Xcode "
date: 2011-04-03 00:36
comments: true
external-url: 
categories: [iOS, programming]

---

经历了上周的one week on rails,这周又切换回iOS了.自从更新了Xcode 4之后,就跑去做支付宝连接和学习Ruby on Rails去了,一直没有碰Xcode. 

Xcode 4相比Xcode 3变化好大,这一周花了N多时间在熟悉和折腾Xcode 4上.

Xcode 4亮点很多,比如Interface Builder现在和Xcode整合在一起了,创建outlet只要拖动到代码里就搞定了,非常方便,让我不禁又重回xib的怀抱.还有其他不少改进.

相关的文章已经很多了.这里就仅记录下碰到的问题,方便后来者:

### 无法自动补全 / 没有语法高亮 的问题:
在build settings里将precompile prefix header设为NO,删掉Derived Data目录(在Organizer里可以找到),等index完之后再看看


### 打开static library时出现 "workspace integrity"错误:

这一般是在打开了引用这个static library的项目的情况下打开这个static library项目导致的.关掉那个项目的xcode窗口即可.

### 按照文档创建workspace后将static library项目拖进workspace结果只是认成是一个文件的问题:

同上.因为这个static library 项目已经在别的xcode窗口中打开了.

### 带有static library的项目找不到lib的问题:

检查static library项目和主项目的building setting是否一致.比如主项目的building setting用的是自建的Distribution而不是Release,那么在static library里也得有这个Distribution名字的setting(并且由于前面的workspace integrity问题当时还没解决,无法打开该library项目,使得这个问题花了我n多时间才解决)

### 带有static library的项目用Organizer提交到app store时无法verify/share/submit的问题:

在static library的building settings里将skip install设为YES

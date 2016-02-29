---
layout: post
title: "My Reading Flow"
date: 2016-02-29 22:22
comments: true
external-url: 
categories: productivity
---


[TOC]

主要用于记录和分享我当前的阅读流程、用到的工具、自动化的方案以及弃用的方案。会随时根据变化而更新，也欢迎大家推荐好用的工具和服务。主要适用于 Mac 和 iPhone 用户。

## 阅读工具

主要用的是 Reeder 和 Instapaper

## 阅读源

![readingSources](http://7xr7zg.com1.z0.glb.clouddn.com/2016-02-28-readingSources.jpg)

*(这里偷懒用 markdown 的顺序图画了下)*
    ```sequence
    Blog->Reeder: via Inoreader
    Reeder->Instapaper:
    PinboardNetwork->Instapaper: 根据 Pinboard Network 的 RSS 利用 IFTTT 存到 Instapaper 上
    Twitter->Instapaper: use Tweetbot's Share to Instapaper
    微信文章->Instapaper: 在 Safari 里打开后 Share to Instapaper
    ```


## 知识库管理
- Evernote
    - 将有价值的文章全文保存至 Bookmark 笔记本中。
- fetching.io
    - 使用其 native 客户端，可对浏览器的浏览历史进行全文搜索，并管理 bookmark

##Highlight 

- Genius + ifttt 自动将 highlight 的内容同步更新到 Evernote 上
    - 目前选用的方案，配合 Mac 上 Chrome 的插件十分方便，不用改变原有的阅读流程。
    - 缺点是手机端支持体验还不是很好，可考虑利用 Instapaper 同步过去
- ~~instapaper  需要付月费~~
- ~~利用 kindle 支持有限~~
- ~~Clippings.io 月费2$~~


## 好文分享流程

目的：将觉得有价值的文章自动化分享到 Pinboard 并同时全文保存到 Evernote
流程：存到 Instapaper，利用它的Like 自动分享到Pinboard 和 Evernote（需要 evernote plus）


可选方案：

- IFTTT 连接 evernote 的 create linked post
    - 缺点是没有全文
- Evernote 的 email post
    - 缺点是需要 evernote plus 月费，每月 18 rmb



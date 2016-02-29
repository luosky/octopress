
---
layout: post
title: "Migrated to Octopress"
date: 2012-07-22 14:46
comments: true
external-url: 
categories: [life, blog]

---


将 blog 从 wordpress 迁移到 Octopress 上了，主要是因为其支持 markdown 。用 markdown 写文章真是太适合理工科的宅男了。

安装完后用 `rake new_post['Title of The Post']` 来新建一篇文章。编辑完后用 `rake generate` 生成 html， 然后 `rake deploy` 即可将文章发布了。

原来的blog改到[photo.luosky.com](http://photo.luosky.com)上，将主要用来放日常用手机拍的照片。

一些迁移时碰到的问题记录如下：
 
Some Tips：

* 用[pow](http://pow.cx/)来本地运行很方便
* rake setup_github_pages时出现undefined method `[]' for nil:NilClass 
	* 一般是因为repository地址你填的是http的地址
	* 填ssh的那个形式如 git@github.com:your_username/your_username.github.com.git 的地址即可
* 带本地图片的markdown上传
	1. 在本地建一个/images 的soft link指向到{octopress}/source/images目录
	2. 将图片放在/images目录下，在markdown文件中以/image/xxx.jpg引用
* When you are filling the values in _config.yml. A space need to be added between the value and the colon after the key.
* add category list 
	* see this [post](http://paz.am/blog/blog/2012/06/25/octopress-category-list-plugin/)



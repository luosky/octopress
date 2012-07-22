---
layout: post
title: "切换到Oh My Zsh"
date: 2012-07-20 10:39
comments: true
external-url: 
categories: programming

---


## Oh My Zsh 的优点
   * 各种补全
      * 目录补全可以只输入目录的中间部分进行补全
      * 命令可以补全参数,并且可以显示参数对应的说明
   * 各种主题
   * 目录切换:
      * d : 显示最近使用过的几个目录,按1-9可以直接切换过去
      * 1-9 : 切换至最近使用过的前n个目录
      * .. 等于 cd ..
      * … 等于 cd ../..
      * mcd xxx  等于  mkdir xxx & cd xxx
      * 使用~xxx快捷目录来通过 cd ~xxx 甚至是 ~xxx 快速进入对应的目录    
         * 在.zshrc里加上 hash -d xxx="/Users/Luosky/Documents/xxx" 
   * 各种插件:
      * extract 直接extract filename ,支持各种tar.gz, bz2, 7z等各种压缩格式
      * terminalapp  让OS X Lion下的Terminal 启动时打开上次的目录
      * osx 
         * pfd    打印finder的当前路径
         * cdf    cd到finder的当前路径
         * pushdf pushd 到finder的当前路径

## 一些问题:

* 换用zsh后出现ssh到服务器上按tab出现 `warning: setlocale: LC_CTYPE: cannot change locale (zh_CN.UTF-8)`的问题
	* 解决:
在~.zshrc内加上
`export LANG=en_US.UTF-8 
export LC_ALL=en_US.UTF-8`


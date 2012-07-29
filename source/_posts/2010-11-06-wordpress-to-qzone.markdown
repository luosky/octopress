---
layout: post
title: "搞定了wordpress同步到qzone"
date: 2010-11-06 00:36
comments: true
external-url: 
categories: [programming, blog]

---


Posted on [2010-11-06][9] by  [luosky][10]

   [9]: http://luosky.com/2010/11/%e6%90%9e%e5%ae%9a%e4%ba%86wordpress%e5%90%8c%e6%ad%a5%e5%88%b0qzone.html (7:45 pm)
   [10]: http://luosky.com/author/luosky (View all posts by luosky)

由于封闭的腾讯从不开放其API,因此无法通过常规的方法同步到qzone.唯一通过外部发表文章的途径只有通过qq邮箱来进行发布.

post2qzone这个插件就是利用这个途径来进行同步.

但很多虚拟主机提供商因为垃圾邮件的关系,都屏蔽了邮件发送的25端口.我用的dreamhost也是如此,ssh上去telnet smtp.qq.com 25 一直都是connection refused.但是ssl加密的端口 587和465端口却可以telnet上去.因此可以利用这点来发布.

看了下﻿post2qzone的源代码,﻿是用的PHPMailerl来发邮件.只需对post2qzone.php 做如下修改即可通过ssl加密来发布邮件了:﻿

在﻿sendPostByPhpMailer函数里加上
    
    
    $mail->SMTPSecure = "ssl";
     $mail->Port       = 587;
    

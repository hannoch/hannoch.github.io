---
layout: post
title:  "GitHub Pages域名绑定2018，别被旧文章误导了"
categories: GitHub
tags:  域名 domain githubpage 绑定
author: Hannoch
---

* content
{:toc}



1.创建GitHub Pages
如果不知道如何创建GitHub Pages，https://pages.github.com/

2.注册域名
到阿里云或者腾讯云买个自己喜欢的域名（.top域名不能作为腾讯域名邮箱）,以下用 example.com 表示你买的域名

3.到项目的设置中添加刚刚买的域名（CNAME 拒绝）
推荐下面的方式，不要用新建文件方式，免得出错

![1.png](https://upload-images.jianshu.io/upload_images/5451635-e15abba26297658d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


往下翻输入你自己的域名，比如：example.com，your site is published at....这里先不用管，直接在domain写上你的域名,然后点save就行。

![2.png](https://upload-images.jianshu.io/upload_images/5451635-c4f01c98c9388060.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


4.域名解析
到域名控制台（阿里云，腾讯云，或者其他域名供应商）找到域名列表点击解析设置，

很多人在这一步出错，或者设置完了但是访问不了（username.github.io 和 example.com 都无法访问）

仅仅添加两个CNAME类型即可，不需要管A类型，或者IP什么的

![3.png](https://upload-images.jianshu.io/upload_images/5451635-fff8afc9091ab547.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


有的旧教程里面各种ping主机，填IP，统统不需要。

主机记录www和@是为了 www.example.com 和 example.com 都能访问到你的页面

这个时候你会发现还是不能访问，因为在等服务商分配DNS（可能不止10分钟），这样配置准没错，去睡个觉醒来就能访问了


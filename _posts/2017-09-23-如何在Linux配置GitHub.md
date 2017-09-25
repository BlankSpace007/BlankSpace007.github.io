---
layout: post
title:  "如何在Linux配置GitHub"
date:   2017-09-23 16:35:55
categories: mediator feature
tags: featured
image: /assets/article_images/2016-09-23/pic.jpg
---

# 如何在Linux配置GitHub
大家用win和Mac系统是怎么把代码提交到github上呢。有些人用sourcetree，有些人用github desktop。但是这些软件都有一个缺点，没有linux版本，那么linux系统该怎么办呢？没错，就是终端，利用命令行去提交代码。那么该怎么配置呢？

准备工作：注册好GitHub的账号
1.打开终端输入下面的命令行
``ssh-keygen -t rsa -C xxxx  ``
xxxx是你的github的注册邮箱
![](/assets/article_images/2017-09-25/pic1.png)

2.从上图可以看到你的key是在/root/.ssh/id_ras.pub里面，所以我们抓取里面的内容。
``cat /root/.ssh/id_ras.pub ``
![](/assets/article_images/2017-09-25/pic2.png)

3.打开GitHub，在setting里面选择SSH and GPG keys里面的New SSH key按钮，title随便填，Key里面就填我们cat出来的key。然后点击Add SSH key按钮。如图
![](/assets/article_images/2017-09-25/pic3.png)

4.测试是否成功：
``sudo ssh -T git@github.com``
成功的话就会出现You’ve successfully authenticated, but GitHub does not provide shell access 。
![](/assets/article_images/2017-09-25/pic4.png)

这个时候我们就可以用开开心心的是敲代码了。
下次我们就来讲下怎么使用git命令玩转github。
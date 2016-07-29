---
layout: post
title:  "Block the website"
date:   2016-01-02
categories: useful
tags: useful time
excerpt: 如何在各个平台上block网站
---
* content
{:toc}

在网上和手机上浪费了大量时间，因为良心呼唤也尝试了一些办法：

## Block the windows website

modify the hosts file in __C:\Windows\System32\drivers\etc__ [^w]

[^w]: http://www.pcworld.com/article/249077/how_to_block_websites.html

## Block IPhone

Active restriction. 但是缺点是我还是会de-restrict然后继续浪费时间。于是乎我想了一个办法，我临时想了个我记不住的密码，然后想解除的时候我发现我忘记密码了。

## Block Mac

那个长着魔鬼头像的selfcontrol很好用。

另一种就是直接修改host文件

sudo nano /etc/hosts

修改类似👇然后输入control+X 退出，输入Y保存

然后输入

sudo dscacheutil -flushcache    进行更新。

> 0.0.0.0 youtube.com

> 0.0.0.0 http://www.youtube.com

## 从源头上做起

注销Facebook，微博（我费了好大的劲儿，硬是把自己搞成卖粉丝的才自杀成功），微信关闭朋友圈（可是我常常忍不住去偷看）。

新的一年多读书，做重要的事情。加油！


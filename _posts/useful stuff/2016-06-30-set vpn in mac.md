---
layout: post
title:  "Set VPN in mac OSX"
date:   2016-06-30
categories: useful
tags: VPN useful
excerpt: 
---
* content
{:toc}

记录两个试过还可以的VPN，一个是 ss-link, 另一个是 天行vpn，前者感觉更快一点。

这两个都可以在官网上找到Mac 客户端，按照教程简单配置一下就连上了。

但是在用ss的时候发现Dropbox无法同步，就在Dropbox里配置了下网络代理，选择 SOCKS5，localhost : 1080, 密码不知道是不是必须的。配好以后重启Dropbox，就可以同步了。

通过这个[blog](https://blog.ss-link.com/archives/208)还可以测试找到最快的节点。
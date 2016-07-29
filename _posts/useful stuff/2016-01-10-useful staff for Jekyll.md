---
layout: post
title:  "Useful skills for Jekyll"
date:   2016-01-10
categories: useful
tags: useful Jekyll
excerpt: 
---
* content
{:toc}

## link to posts from pages[^1]
Jekyll has a built in function that allows you to internally link or cite back to posts on your website.


![link post](/images/posts/link-post.png)

if you want to make hyperlinks.

**注意：后缀.md一定不能加！否则会编译失败！**

[^1]: http://stackoverflow.com/questions/31480030/jekyll-link-to-posts-from-pages

## Archive posts using tags[^2]
This works for me.

[^2]: http://blog.lanyonm.org/articles/2013/11/21/alphabetize-jekyll-page-tags-pure-liquid.html

## Center images and add captions
在网上搜了很久试了很多办法都不管用。就采用了最直白的傻办法：

```
<center>
<img class='center-image' src="/images/posts/racks-of-computing-nodes.png" width="300" alt="racks of computing nodes" align="middle" >
<br>
racks of computing nodes
</center>
```

## Reference



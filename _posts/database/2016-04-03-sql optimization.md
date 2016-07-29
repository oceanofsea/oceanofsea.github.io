---
layout: post
title:  "SQL optimization"
date:   2016-04-03
categories: database
tags: database sql optimization
excerpt: 
---
* content
{:toc}

## 避免全表扫描

* 避免在 where 子句中对字段进行 null 值判断
	- 可以设置默认值代替null
	- e.g. ```select id from t where num = 0``` instead of ```select id from t where num is null	```
* 避免在 where 子句中使用 != 或 <> 操作符
* 应尽量避免在 where 子句中使用 or 来连接条件，如果一个字段有索引，一个字段没有索引，将导致引擎放弃使用索引而进行全表扫描
	- 用union代替or
* 少用in和not in，用exists来代替

## Update
Update 语句，如果只更改1、2个字段，不要Update全部字段，否则频繁调用会引起明显的性能消耗，同时带来大量日志。

## Other
* 启动查询缓存，这样对于相同的query就不需要重复劳动
* 当只要一行数据时使用 LIMIT 1, 这样找到符合要求的数据就停止搜索
* 对常搜索的字段建立index
* 避免select *, 养成需要什么取什么的好习惯
* 如果attribute的值是固定的，use enum instead of varchar





## Reference
* http://www.cnblogs.com/yunfeifei/p/3850440.html
* http://coolshell.cn/articles/1846.html
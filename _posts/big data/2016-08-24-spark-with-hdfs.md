---
layout: post
title:  "Spark with hdfs notes"
date:   2016-08-24
categories: BigData
excerpt: 
tags: Useful Linux
---

Copy file to HDFS

```
hdfs dfs -put input
```

> Note that if the filename contains ":", the hdfs will not store it.

check the file in hdfs 

```
hadoop fs -ls
```

load data to create RDD

```
val myfile = sc.textFile("hdfs://namenode_host:8020/path/to/input")
```


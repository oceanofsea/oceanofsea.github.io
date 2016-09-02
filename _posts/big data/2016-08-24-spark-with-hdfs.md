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
hadoop fs -put <localsrc>  <HDFS_dest_Path>
```

Copy hdfs files to local

```
hadoop fs -get <localsrc>  <HDFS_dest_Path>
```

See contents

```
hadoop fs -cat <path[filename]>
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

You can find your hadoop name node address in hadoop UI, 
in my case, open arda UI and go to vertical, 
```http://***.181:50070/```,
you will find the title ```Overview 'Gondolin-Node-081:9000' (active)```,
here, ```Gondolin-Node-081:9000``` is your namenode host.

Note that, by default, when you write to hdfs, it will put it into user/root/

So the path to your hdfs file should be ```hdfs://Gondolin-Node-081:9000/user/root/filePath```.



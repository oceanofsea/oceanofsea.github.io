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


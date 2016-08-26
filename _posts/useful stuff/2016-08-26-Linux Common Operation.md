---
layout: post
title:  "Ubuntu Common Operation"
date:   2016-08-26
categories: useful
tags: Ubuntu useful
excerpt: 
---
* content
{:toc}

# Set system path

```
export JAVA_HOME=/home/arda/tools/jdk1.8.0_101
export PATH=$PATH:$JAVA_HOME/bin
```

# File Transfer

```sftp username@ip```

pwd: print working directory

lpwd: local working directory

get remoteFile [localFile]: get remote file to local

get -r someDirectory: get the directory content recursively

put localFile

put -r localDirectory

# File (de)compression

```tar -zxvf filename.tar.gz```

> (=tape archiver) Untar a tarred and compressed tarball (*.tar.gz or *.tgz) that you downloaded from the Internet.

```tar -xvf filename.tar``` 

> Untar a tarred but uncompressed tarball (*.tar).

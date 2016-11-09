---
layout: post
title:  "Intellij Useful"
categories: useful
tags: Intellij useful
excerpt: 
---
* content
{:toc}

# cannot find the project structure

Sometimes when opening a project, the folders are not shown. In this case, you should goto

```
project setting -> modules -> add the whole folder as module
```

# Index files so slow

```
File -> Invalidate Cache and restart
```

Also if the intellij is so slow, modify the memory config

```
-Xms128m
-Xmx8192m
-XX:MaxPermSize=1024m
```

On a Mac, this file is located in this path: 

(For IntelliJ 2016 or Higher on Mac) 

```
/Applications/IntelliJ IDEA.app/Contents/bin/idea.vmoptions
```

On a linux, this file is in 

```
~/.IdeaIC2016.2/idea64.vmoptions
```

Reference: http://stackoverflow.com/questions/20545435/why-is-the-new-version-of-intellij-idea-so-slow

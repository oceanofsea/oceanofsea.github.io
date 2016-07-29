---
layout: post
title:  "Useful skills for Mysql"
date:   2016-01-10
categories: database
tags: database mysql useful
excerpt: 
---
* content
{:toc}

## Import & Export [^2]

不用进入sql里面，直接在command line打开时的状态下输入就可以了

### Export 

```
mysqldump -u [username] -p [database_name] > [dumpfilename.sql]
```

### Compress (Optional)
When the export is complete, it’s helpful to compress the file to make it transfer to the new server faster. To do so, run the following command:

```
tar zcf dumpfilename.tar.gz dumpfilename.sql
```

replacing dumpfilename with your actual file name.

Once the file is on the new server, start be decompressing it using the following command:

```
tar zxf dumpfilename.tar.gz
```

### Import
To begin the import process, use the command:

```
mysql -u [username] -p [database_name] < [dumpfilename.sql]
```

[^2]: [3 ways to import and export a MySQL Database](http://www.itworld.com/article/2833078/it-management/3-ways-to-import-and-export-a-mysql-database.html)

## Reset MySQL root password[^1]
Here’s a convenient way to reset your root password when you lose it.

```/usr/local/mysql/bin/mysqladmin -u root -p password```

The command line will give you a similar question:

```Enter password:```

Don’t add anything and you will push asked for your new password:

```New password:```<br>
```Confirm new password:```

Voila! Your MySQL root password has been reset!

## Access denied error

写着写着程序，突然就报了这个错！Access denied for user 'root'@'localhost'!!! re-install mysql 也不管用！折腾了好久，总算是找到了解决方案。[^4]

[^4]: http://www.euryugasaki.com/archives/853

下面是解决方案：
1、先在系统偏好设置中关闭MySQL服务；
2、在终端中输入

```
sudo su
mysqld_safe --skip-grant-tables --skip-networking &
```

这时便能越过权限表，直接登陆MySQL了。
3、新建一个终端，输入

```
mysql -u root
```

4、 在MySQL中修改root用户密码即可：

```
mysql> UPDATE mysql.user SET password=PASSWORD(’新密码’) WHERE User=’root’;
mysql> FLUSH PRIVILEGES;
```

然而，This didn't work with me!

就在这时，我遇到了救星！[^3]
[^3]: http://stackoverflow.com/questions/30692812/mysql-user-db-does-not-have-password-columns-installing-mysql-on-osx

> There is no field named 'password', the password field is named ' authentication_string'. !!!

```
update user set authentication_string=password('1111') where user='root';
```

Lesson:

* 软件保持兼容和命令的一致是多么重要
* 无论如何变化如何棘手，总是有充满智慧和热心的人出现！

## Create a user
上一个问题的另一种解决办法就是create a new user

To create a new MySQL user:

Login to MySQL in Terminal: ```mysql -u username```

In the prompt:

```
CREATE USER 'admin'@'localhost';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost';
```

## Restart Computer
尝试了各种办法，最好的一种是重启💻！




## Reference
[^1]: https://gistpages.com/posts/reset_mysql_root_password_on_mac_os
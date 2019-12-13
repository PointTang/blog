---
title: Android碎片知识
date: 2019-12-13 18:32:00
tags: [Android]
category: [tip]
---

Android碎片记录。

<!-- more -->

# SQLite文件为什么那么大？

引用自别人的博客：https://www.cnblogs.com/liaocheng/articles/6182976.html

总结以下就是，SQLite是变长存储，有个功能叫vacuum，默认不会自动运行。不开启这个auto-vacuum时，从SQLite删除数据后，未使用的磁盘空间会被添加到一个内在的“空闲列表”中，用于存储下次插入的数据，提高效率，磁盘空间没有丢失，但也不向操作系统返回磁盘空间，这就导致删除数据乃至清空整个数据库后，数据文件大小没有缩小。

解决方法：

一是手动执行vacuum命令，执行方法是在sql命令行输入```vacuum;```无需参数，即可清空空闲列表。需要使用两倍于数据库文件的空间，也需要耗时。

二是在建表前开启自动清空，这样在每次删除时会数据库文件会自动收缩大小。但这种方式，只会从数据库文件中截断空闲列表的页，不会回收数据库中的碎片，也不会向vacuum命令那样重新整理数据库内容。实际上，由于需要在数据库文件中移动页，auto-vacuum会产生更多的碎片。

```
cmd.CommandText = "PRAGMA auto_vacuum = 1;"
cmd.ExecuteNonQuery()
```

要使用auto-vacuum，需要一个前提条件：数据库中需要存储一些额外的信息以记录它所跟踪的每个数据库页都找回其指针位置。 所以，auto-vacumm 必须在建表之前就开启。在一个表创建之后，就不能再开启或关闭 auto-vacumm。

两种方式在删除时都会有.db-journal后缀名的临时文件产生。
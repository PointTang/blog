---
title: PHP（二）时间日期及数据库
date: 2019-9-12 16:57:37
tags: [PHP,SQL]
categories: [tip]
---

本篇记录PHP使用过程中用过的时间日期处理和数据库相关代码。

<!-- more -->

PHP的数据库操作参考手册点[这里](https://www.runoob.com/php/php-ref-mysqli.html "PHP 5 数据库MySQLi")。

PHP的时间日期操作参考手册点[这里](https://www.runoob.com/php/php-date.html "PHP时间日期")。

# 什么是时间戳 #

时间戳是一个时区无关的量。

地球上分为24个时区，每个时区都有各自的时间，但同一时刻所有时区距离某一标准时间的差值是一致的，这个标准差值就是时间戳。

标准时间一般认为是格林威治时间1970年01月01日00时00分00秒，即0区时间1970-01-01 00:00:00，对应到北京时间是+8区时间1970-01-01 08:00:00。

PHP和MySQL中的时间戳都是10位，以秒为单位，在部分其他系统中，时间戳是13位，以毫秒为单位。

时间戳最远表示的时间是2038年，十位时间戳2147483647表示格林威治时间2038-01-19 11:14:07。

# 默认时区区别 #

PHP的时区默认是0区格林威治标准时区。

MySQL的时区默认是系统设置时区。

# PHP获取当前时间戳 #

PHP使用time()方法获取当前时间戳，适用于PHP 4+。

# PHP获取当前日期时间 #

## 获取当前时间 ##

使用```date('Y-m-d H:m:s')```方法。这个方法拿到的永远是0时区的时间。

## 获取当前时区的时间 ##

1. 先设置date环境变量，参考 [date_default_timezone_set()](https://www.runoob.com/php/func-date-default-timezone-set.html) 。
   设置东八区使用:```date_default_timezone_set("PRC"); ```
   其中PRC是中华人民共和国（The People's Republic of China）的简称。

2. 再使用date方法获取当前时间：```date('Y-m-d H:i:s')```
   显示格式为*2019-09-15 17:45:27* 

# MySQL写入时间戳 #

MySQL在建表时把字段类型设置成*timestamp*，默认值设为*CURRENT_TIMESTAMP*即可保存时间戳。

勾选根据当前时间戳更新，则每次更新记录时都会更新时间戳，不勾选，则记录创建时的时间戳。两种情况下建表SQL语句示例如下：

```sql
CREATE TABLE `t_auth_v0` (
  `F_MACADDRESS` varchar(12) NOT NULL,
  `F_INIT_TIME` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `F_TIMES` int(4) NOT NULL,
  PRIMARY KEY (`F_MACADDRESS`)
)

CREATE TABLE `t_auth_v0` (
  `F_MACADDRESS` varchar(12) NOT NULL,
  `F_INIT_TIME` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `F_TIMES` int(4) NOT NULL,
  PRIMARY KEY (`F_MACADDRESS`)
)

```

# MySQL读取时间戳

MySQL中直接查询timestamp字段时，如果不设置查询Unix时间戳，会按照系统当前时区把存储的timestamp类型转换成可读的时间。时间格式示例： *2019-09-11 12:22:23* 

示例查询语句：

```
SELECT F_MACADDRESS, F_INIT_TIME, F_TIMES FROM t_auth_v0 WHERE F_MACADDRESS='000000000000'
SELECT F_MACADDRESS, UNIX_TIMESTAMP(F_INIT_TIME), F_TIMES FROM t_auth_v0 WHERE F_MACADDRESS='000000000000'
```

MySQL在时间戳字段上有```UNIX_TIMESTAMP```方法和```FROM_UNIXTIME```方法。由于我们时间戳均为自动生成，此处不再赘述。

---
title: SQL语句记录
date: 2018-05-29 15:21:39
tags: [SQL]
categories: [tip]
---

本篇用于记录SQL语句的不常见玩法，基本的增删改查不写。

<!-- more -->

# REPLACE

这次遇到的需求是这样的：
新到一批数据，批量导入了数据库，然后老大后悔了，原定的某个字段以NSW开头，现在想批量改成NWG开头，后面的字符不变。
这个需求本来是打算写一段SQL代码，定义变量，然后取出来原来的，改掉，再重新插入。
然后有哥们提示可以用Replace函数。

这个函数很简单，```replace(列名, '原关键字', '新关键字')```，可以套用在select和update里。
套在select里，将只对查询结果进行replace，例如```SELECT REPLACE (FU, 'NSW', 'NWG') FROM t_c;```将在t_c表的查询结果里把FU字段的NSW替换为NWG，不影响原数据。
套在update里，将对原数据进行replace，例如```UPDATE t_s SET FU = REPLACE (FU, 'NSW', 'NWG');```将对t_s表里的FU字段的NSW全部替换为NWG。

replace还有其他的用法，等需求到了用的时候再说，至少先明白有这个关键字/函数。

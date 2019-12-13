---
title: Git使用问题记录
date: 2019-12-13 18:26:40
tags: [git]
category: [guide]
---

Git使用过程中各种各样的问题和想法。

<!-- more -->

# AS输错Git账号密码

Android Studio里的Git插件，第一次测试登入的时候输账号密码输错了，在Android Studio里就没有地方可以重新输入了。因为这个账号实际上保存到了Windows系统里，再做测试连接的时候，Android Studio会去读系统里保存的账号密码。

修改方式是：电脑——控制面板——用户账户——管理Windows凭据，找到git账号，编辑修改后，在Android Studio里重新测试连接。
---
title: 使用CircleCI自动部署项目
date: 2019-11-01 17:46:50
tags: [hexo]
category: [guide]
---

本篇介绍如何使用CircleCI自动部署博客到Github。

<!--more-->

有一天我需要更换电脑。然后我发现我github上的博客没有保存源码，只保存了自动生成的工程。

而且我想了想，我的博客部署一次好麻烦啊。

然后，在同事的帮助下，我尝试了Travis-CI和Circle CI，结果，这个成功了。

接下来，我只记录一下这个成功案例。



发现的第一个问题：

提交后theme是空的。

检查后看到，github仓库里，这个空的theme和默认的landscape图标do不一样。

搜索一波发现，这个玩意是一个submodule。然而又找不到submodule的配置在哪里。

但是我找到了怎么解除submodule。

```
$ git ls-files --stage | grep 16000
```

这个命令可以列出来所有的submodule，虽然我不知道这是什么玩意。

```
$ git rm --cached themes/3-hexo
```

用这个命令先把原来的删掉再说。


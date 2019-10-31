---
title: Hexo博客搭建（二）
date: 2019-08-02 16:53:28
tags: [hexo]
category: [guide]
---

本篇介绍Hexo增加新文章的基础命令。

<!-- more -->

# 新增名为Hello的文章

命令：
```
hexo new hello
```
该命令会在 *source/_posts* 目录下生成一个 *hello.md* 文件

# 给文章配置属性

```
title: Hexo博客搭建（二）
date: 2019-08-02 16:53:28
tags: [hexo]
category: [guide]
```
其中：

*tags* 对应上一篇里的 *tags* ，文件目录在 *source/tags/* 。如果需要多个标签，则用逗号分隔，如：
```
tags: [PHP,SQL]
```
*category* 对应上一篇里生成的 *categories* ，文件目录在 *source/categories/* 。

# 实用命令
```
hexo clean # 清除已经生成的缓存文件，这个操作会删除public目录下所有的文件。
# 因此推荐把头像也在source目录底下放一份。g命令会自动生成到public目录去。
hexo g # generate, 生成静态文件，从source到public。
hexo s # 启动本地服务器
hexo d # 部署到远程服务器，如github。
```


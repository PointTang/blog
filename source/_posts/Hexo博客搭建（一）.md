---
title: Hexo博客搭建（一）
date: 2018-05-21 16:08:33
tags: [hexo]
categories: [guide]
comments: false
---

本篇记录如何从零开始搭建一个博客，以及我起手遇到过的问题。

[Hexo官方向导](https://hexo.io/zh-cn/docs/index.html "Hexo官方向导")讲解了如何去搭建环境，这里照着教程做就好。其中可能遇到的问题我在[CSDN博客](https://blog.csdn.net/xiaobudian99/article/details/80299569 "CSDN博客")里也讲了很多，这篇会挑一些讲。

<!-- more -->

# 配置及部署 #
在安装完环境、init出第一个博客项目之后，有一些配置需要手动更改，文件是**根目录**的**_config.yml**文件。这个文件后面会在其他地方也看到，注意看清楚目录位置。

这个文件可以改一些东西，首先是第一部分，Site，可以修改标题、副标题、说明等等大面上的内容，改过之后至少一眼看着这个博客就是你自己的了。

下面大多数保持不动，然后再修改一个地方，最后的deploy，这个地方是项目部署的位置，我在乐乐的代码里看到，他一个项目可以部署到两个地方，这里看个人需要。我在他的代码里看到，如果部署两个，只需要在每一个的type前面加一个减号“-”就好了。

```
deploy:
  type: git
  repository: git@github.com:PointTang/PointTang.github.io.git
  branch: master

```

博客项目搭好以后，在本地使用**hexo s**命令启动，会默认启动到4000端口，然后可以使用localhost:4000请求访问。

启动好以后，想再放到github去，需要先停掉本地的服务，再去命令行**hexo g**和**hexo d**。

有一点要说的是，hexo clean命令，会清空根目录的/public目录，然后hexo g命令又会重新生成/public目录。

# 自定义准备工作#
不管用什么主题，有一些模块大体是通用的。比如分类、标签、关于我之类的目录，在项目初始化的时候并不会自动创建。

准备这些文件有两种方式，一种是到项目source目录底下手动创建文件夹，再手动创建index.md文件，另一种就是我喜欢的命令行方式。

我创建了“关于我”“分类”“标签”这三个东西：
1. 命令```hexo new page "tags"```创建标签md文件，然后在根目录/source/tags/index.md中修改type属性为```"tags"```。
2. 命令```hexo new page "categories"```创建分类md文件，然后在根目录/source/categories/index.md中修改type属性为```"categories"```。
3. 关于我没有直接用命令行创建，我在根目录/source/下直接新建了about文件夹，在about文件里新建了index.md文件，然后修改页面内容。

# 主题更换#

总是不喜欢用系统默认主题，官方主题列表在[这里](https://github.com/hexojs/hexo/wiki/Themes "官方主题站")，也说明了怎么更换主题。

主题不好挑的话，随便[知乎](https://www.zhihu.com/question/24422335 "有哪些好看的Hexo主题推荐")或者别的搜一下就能看到有推荐，还是排排列好图的那种。

主题配置在哪里改，这是另一个**_config.yml**文件了，在**/themes/jacman/**目录底下。这个文件修改的时候，看着挑选的主题的修改说明就好了。

有图标什么的修改更换，也都放在/source目录底下新建一个img文件夹，可以换主题的时候通用，还能不被clean命令清除掉。

# 发布上线 #
所有的更改做完都可以通过本地hexo s直接预览，预览成功后停掉本地服务，使用hexo d和hexo g命令上线到github去，就可以看到更改了。

# 后记 #
成长，就从这里开始吧。做宇宙中一个小点，也要做更好的点。

I'm Point. 

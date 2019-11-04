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

然后，在同事的帮助下，我尝试了Travis-CI和Circle CI，结果，Circle CI成功了。Travis-CI有一个坑爹的加密一直搞不对。

接下来，我只记录一下这个成功案例。



github新建一个blog仓库来放源文件。原来的pointtang.github.io用来放网页。

把旧电脑上的博客源码拷过来，新电脑上排排装环境，然后提交到blog仓库去。

发现的第一个问题：

我在新电脑上移动了一次目录，结果是本地的不能运行了。

我知道是node js里面什么东西依然在往我旧目录放，但是我找不到在哪里设置。

然后，卸载重装node js。电脑用户目录/AppData/Romaing/npm两个都删掉。博客项目根目录下的node_modules删掉。

重新走npm install流程。

发现的第二个问题：

提交后theme是空的。github仓库里，这个空的文件夹和默认的landscape图标都不一样。

这玩意是一个submodule，但是由于我原来是旧版本的git，用git clone命令加进去的主题，submodule就不正常了。

我找到了怎么解除submodule。

先把主题底下的_config.yml保存起来。

然后在github上，fork一个主题工程，分支什么的改好。

然后在本地解除错的submodule。先把他找出来，看看在不在。

```
$ git ls-files --stage | grep 16000
```

这个命令可以列出来所有的submodule。如果有一行themes/3-hexo，那就是旧的，错的。然后先把它删掉。

```
$ git rm --cached themes/3-hexo
```

下一步，用submodule来管理。

```
$ git submodule add https://github.com/PointTang/hexo-theme-3-hexo.git themes/3-hexo
```

再下一步，把主题的yml文件放回去。放到themes/3-hexo目录底下。

再下一步，cd到themes/3-hexo目录，把这个主题的内容提交一哈。然后push到远程仓库。

这一步过了，去github上检查，就能看到fork的仓库里最新的提交了。

再下一步，cd回blog目录，把整个工程提交一哈，这样关联的submodule就是最新的commit id了。

然后，开始准备用Circle CI。

先去操作一下github。

在右上角Settings -> Develop Settings -> Personal Access Token，新建一个token用来部署。把这个token复制存起来备用。

勾选repo有关的就好。

再去操作一下Circle CI。

先到https://circleci.com/，右上角有一个Go to app的，用github账号登录。

然后关联一下blog工程。

左边第一个选Jobs，然后看到blog旁边有一个设置按钮，点进去，翻到环境变量Environment Variables，把这几个配上：

1. GH_EMAIL 填github的邮箱
2. GH_USER  填github的用户名
3. GH_REF   填github要作为页面显示的那个项目地址，不带前面的https://
4. GH_TOKEN 填刚刚生成的token

再写一下本地工程里的Circle CI脚本。

工程根目录建一个文件夹，名字是.circleci，文件夹里面有一个文件，名字是config.yml。

文件内容照着抄。

```
version: 2
jobs:
  generate-page:
    docker:
      - image: node:8.11.1
    steps:
      - checkout
      - run:
          name: 安装hexo
          command: npm install -g hexo-cli
      - run:
          name: 安装依赖
          command: npm install
      - run:
          name:  更新关联
          command: |
            echo "submodule init."
            git submodule init 
            echo "submodule update."
            git submodule update
      - run:
          name:  生成页面
          command: hexo generate
      - run:
          name: 部署Github Pages
          command: |
            cd public
            ls
            git init
            git config user.name "$GH_USER"
            git config user.email "$GH_EMAIL"
            git add .
            git commit -m "update"
            git push --force --quiet "https://$GH_TOKEN@$GH_REF" master
            echo 'deploy success'
workflows:
  version: 2
  build-deploy:
    jobs:
      - generate-page:
          filters:
            branches:
              only: master
```

写完提交一下。

然后去CircleCI查看Job，会看到这次提交的自动运行。


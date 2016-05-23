---
layout: post
title: git-flow 笔记
date: 2014-12-04 11:21:04
tags: Git
---

git-flow 是一个 git 扩展集，按照 Vincent Driessen 的分支模型提供高层次的库操作，git-flow 提供了出色的命令帮忙以及输出提示。gt-flow 是一个基于归并的解决方案，他并没有提供重置特性分支的能力。

像我想在在 Mac 上用的 Sourcetree 是一个不错的 git 客户端，已经集成了 git-flow 的功能，向大家推荐这个客户端，看 diff 还是不错的。

##安装

前提是你已经安装好了 git 。

- OSX 安装：$ brew install git-flow

- Linux安装：$ apt-get install git-flow

- windows(cygwin)：$ wget -q -0 - --no-check-certificate https://github.com/nvie/gitflow/raw/develop/contrib/gitflow-install.sh | bash

##安装完了就开始吧

###初始化
```
git flow init
```

期间要写一些命名什么的，全部用默认值吧

###开发新特性

一般为我们在 develop 库的分支里开发新功能特性，也就是说新特性的开发基于 develop 分支的，通过下面的命令开始开发新特性

`git flow feature start MYFEATURE` 这个操作创建了一个基于 'develop'的特性分支，并且换到这个分支下。

###完成新特性
完成新特性包含的工作有：

- 合并 MYFEATURE 分支到 'develop'
- 删除这个新特性分支
- 切换回 'develop' 分支

###发布新特性
有时候你也许是和别人一起合作开发这个新特性，发布新特性分支到远程服务器，其他的成员也可以使用这个分支。可以使用下面的命令行

git flow feature publish MYFEATURE

###拉取一个已经发布的特性分支
如果你要拉取一个其他队友发布的新特性分支，并签出远程的变更，可以使用下面的命令行

git flow feature pull MYFEATURE


##开发一个 release 版本
这个版本应该支持一个新的用于生产环境的发布版本，允许修正小问题，病危发布版本准备元数据

###开始准备 release 版本
开始准备 release 版本，使用 git flow release 命令，它从‘develop’分支开始创建一个 release分支

git flow release start RELEASE [BASE]

你可以选择提供一个 [BASE] 参数，即提交记录的 sha-1 hash 值，来启动 release 分支。这个 提交记录的sha-1 hash 值必须是 develop 分支下的

创建 release 分支后立即发布允许其他用户向这个 release 分支提交内容是个明智的做法。
  `git flow release publish RELEASE`命令发布新特性，`git flow release track RELEASE`命令签出 release 版本的远程变更
  
###完成 release 版本

完成 release 版本是一个 git 分支操作，他执行下面几个动作

- 归并 release 分支到 master 分支
- 用 release 分支名做 Tag
- 归并 release 分支到 develop 
- 移除 release 分支

通过命令行 `git flow release finish RELEASE` 执行

###开始 gitflow 热点修复

像其他 git flow 命令一样，热点修复分支开始自：

`git flow hotfix start VERSION [BASENAME]`

VERSION 参数标记着修正版本。
你可以从 [BASENAME] 开始，`[BASENAME]`为 finish release 时填写的版本号

###完成热点修复

当完成热点分支，代码归并回 develop 和 master 分支。相应的 master 分支打上修正版本的 TAG.

`git flow hotfix finsh VERSION`

整体的命令结构流程如下图：
![](http://dyy.im/wp-content/uploads/2014/12/git-flow-commands.png)

以上也只是 git flow 比较重要的命令，其他你常用的 git 命令也可照用。





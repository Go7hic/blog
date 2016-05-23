---
layout: post
title:  使用 Atom 前端开发
date: 2015-10-09 11:21:04
tags: JavaScript
---

编辑器之战是程序员之间永不停歇的战争，如果说 vi 和 Emacs 是所有程序员之间的战争，那么 sublimeText 和 Atom 则可能是 web 开发届的编辑器之战。
你要做个调查做前端开发的用什么编辑器最多，我相信现在还是 sublimeText 最多，但是 Atom 作为 Github 出品的编辑器也收买了大量的前端人员的心，越来越多的前端
开始从 sublimeText 转向 Atom。论优雅 Atom 有过之而无不及，论插件扩展， Atom 则更加灵活了，而且还是用前端语言来写的，厉害一点的前端完全可以自己写自己需要的 Atom 插件，但是 sublimeText 的扩展是 python 写的，而且安装扩展的源好像被封了（不知道现在好了咩），要改 Host 才能安装，且不论自带的 markdown 功能完全可以当 markdown 编辑器使用，自带补全也比 sublimeText强的多，当然这是说自带，插件实现的话两者都很强。
当然 Atom 一直被诟病的一点就是启动慢，但是我相信程序员的电脑配置不会太差吧，这点速度差别还是可以接收的，而且我用了一段时间的 Atom 完全不觉得这有啥影响，完全可以忽略这启动速度的差别。
这里再提一下 webstrom 这个 IDE，虽然是个 IDE ,但是在前端圈也有人喜欢用，因为提示什么的都很智能，但是我想说 Atom 自带 jshint 效果，完全可以智能检查提示你的代码
，所以这个完全可以放弃 webstrom。

好了，不扯那么多了，作为一名前端，从最开始的 vim，到后面一直用 sublimeText2/3，到现在使用 Atom。我觉得好马配好鞍，程序员肯定就的用自己最喜欢的最好用的编辑器来写代码，而且 sublimeText 和 Atom 的很多快捷键基本相同，所以根本没啥学习成本。

安装使用很简单，我也不写教程了，这里记录一下我现在用到的一些 Atom 插件吧，直接在终端 `apm list` 即可看到。

```
├── angularjs@0.3.3
├── atom-beautify@0.28.16
├── atom-jade@0.3.0
├── atom-jshint@2.0.0
├── atom-material-ui@0.6.4
├── css-snippets@0.9.0
├── docblockr@0.7.3
├── emmet@2.3.13
├── file-icons@1.6.11
├── git-plus@5.4.7
├── html-to-string@0.2.3
├── jshint@1.8.1
├── minimap@4.16.0
├── open-in-browser@0.4.6
├── react@0.12.10
├── set-syntax@0.3.0
└── toggle-quotes@0.11.3
```

还有一些快捷键记录：

```
ctrl + alt + b

格式化代码 (来自git beautify)

ctrl + d

在有选中的状态下，快速选中下一个。

ctrl + k

在有选中的状态下，跳过当前选中。

alt + g up/down

到上一个/下一个修改处 (来自git diff)

ctrl + up/down

选中行上移、下移。

ctrl + shift + a + a

添加所有已变更的文件到git并commit (来自git puls)

ctrl + ,

打开设置面板

ctrl + g

进入行号跳转面板。

ctrl + t

进入文件跳转面板。

ctrl + r

进入变量、函数跳转面板。

ctrl + shift + up/down

向上或者向下复制光标。

ctrl + shift + b

运行编辑区域的代码（来自script）。

ctrl + alt + t

打开一个新的终端窗口，起始目录为项目根目录。（来自atom-terminal）
```

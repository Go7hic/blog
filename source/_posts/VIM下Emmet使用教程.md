---
layout: post
title: VIM下Emmet使用教程
date: 2014-05-26 11:21:04
tags: Vim
---

Emmet 项目原来叫 Zen Coding。由俄罗斯前端开发工程师 Sergey Chikuyonok 开发，后来在 Google Code 上释出 Zen Coding 项目。2012年的时候，项目启用新名称Emmet。

Emmet 官方支持很多软件，比如 Sublime Text、Notepad++、Dreamweaver、Eclipse、Adobe Brackets 等。Emmet.vim 并非 Emmet 亲生，它由日本 Yasuhiro Matsumoto 开发。

下载 Emmet.vim

你可以从两个地址下载，一是 [Vim 插件站点](http://www.vim.org/scripts/script.php?script_id=2981)，一是 [Github](https://github.com/mattn/emmet-vim/)。

安装 Emmet.vim

将下载的压缩包解压到 .vim 目录下：
```
cd ~/.vim
unzip emmet-vim.zip
```
如果你使用 pathogen.vim 管理 Vim 插件：
```
cd ~/.vim/bundle
unzip /path/to/emmet-vim.zip
```
或者直接从 Github 库克隆一份：
```
cd ~/.vim/bundle
git clone http://github.com/mattn/emmet-vim.git
```
个人建议通过 Pathogen 或 Vundle 来安装管理 Vim 插件。

使用 Emmet.vim

以下内容译自 Emmet.vim tutorial，感谢作者。

1. 展开缩略词

键入缩略词组 div>p#foo$*3>a 然后按快捷键 <C-Y>, – 指按 <Ctrl-y> 后再按逗号。
```
<div>
    <p id="foo1">
        <a href=""></a>
    </p>
    <p id="foo2">
        <a href=""></a>
    </p>
    <p id="foo3">
        <a href=""></a>
    </p>
</div>
```
2. 包入

如下内容：
```
test1
test2
test3
```
按大写的 V 进入 Vim 可视模式，“行选取”上面三行内容，然后按键 ‘<C-Y>,‘，这时 Vim 的命令行会提示 Tags:，键入ul>li*，然后按 ENTER。
```
<ul>
    <li>test1</li>
    <li>test2</li>
    <li>test3</li>
</ul>
```
而假如输入的 tag 是 ‘blockquote’，则结果会变成下面这样。
```
<blockquote>
    test1
    test2
    test3
</blockquote>
```
3.插入模式下根据光标位置选中整个标签

按 `‘<C-Y>D‘`

4.插入模式下根据光标位置选中整个标签内容

按 `‘<C-Y>D‘`

5.跳转到下一个编辑点

插入模式下按 `‘<C-Y>N‘`

6.跳转到上一个编辑点

插入模式下按 `‘<C-Y>N‘`

7.更新图片大小

移动光标到 img 标签。
```
<img src="foo.png" />
```
然后按 `‘<C-Y>I‘`
```
<img src="foo.png" width="32" height="48" />
```
注：这个据个人使用经历，仅适用本地图片，互联网上的图片并无法取得其大小。

8.合并行

选择包含 `‘<li>’ `的行
```
<ul>
    <li class="list1"></li>
    <li class="list2"></li>
    <li class="list3"></li>
</ul>
```
然后按 `‘<C-Y>M‘`
```
<ul>
    <li class="list1"></li><li class="list2"></li><li class="list3"></li>
</ul>
```
9.移除标签对

移动光标到块中
```
<div class="foo">
    <a>cursor is here</a>
</div>
```
在插入模式下按 `‘<C-Y>K‘`。
```
<div class="foo">

</div> 
```
再次按 `‘<C-Y>K‘ `则上述连 div 标签块都没了。

10.分割/合并标签

移动光标到块中
```
<div class="foo">
    cursor is here
</div>
```
在插入模式下按 `‘<C-Y>J‘`。
```
<div class="foo"/>
```
再次按 `‘<C-Y>J‘`。
```
<div class="foo">

</div>
```
11.切换注释

移动光标到块中
```
<div>
    hello world
</div>
```
插入模式中按 `‘<C-Y>/‘`。
```
<!-- <div>
    hello world
</div> -->
```
再按 `‘<C-Y>/‘`。
```
<div>
    hello world
</div>
```
12.从 URL 地址生成锚

将光标移至 URL

http://www.google.com/
按 ‘<C-Y>A‘
```
<a href="http://www.google.com"></a>
```
13.从 URL 地址生成引用文本#

移动光标到 URL

http://github.com/
按 `‘<C-Y>A‘`
```
<blockquote class="quote">
    <a href="http://github.com/"></a><br />
    <p>...</p>
    <cite>http://github.com/</cite>
</blockquote>
```
14.安装 Emmet.vim

见文章头部。

15.为你所用的语言启用 Emmet.vim#

你可以为你用的语言自定义行为。
```
# cat >> ~/.vimrc
let g:user_emmet_settings = {
\ 'php' : {
\ 'extends' : 'html',
\ 'filters' : 'c',
\ },
\ 'xml' : {
\ 'extends' : 'html',
\ },
\ 'haml' : {
\ 'extends' : 'html',
\ },
\}
```
余话

除开以上帮助，你还可以按冒号进入 Vim 命令行模式，然后输入 help emmet 在新窗口中调用 Emmet 的帮助内容。

Emmet 在其他编辑器的触发快捷键一般是 TAB 或 CTRL+E，如果你更习惯它们，也可以在 .vimrc 文件中加入下一行命令来修改它的触发快捷键：

let g:user_emmet_expandabbr_key = '<Tab>'
这样就可以按 TAB 来扩展了。

附:

ZenCoding cheatsheet





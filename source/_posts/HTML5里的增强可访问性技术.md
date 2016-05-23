---
layout: post
title: HTML5里的增强可访问性技术
date: 2013-11-20 11:21:04
tags: HTML5
---


目标：使用html5来提升使用辅助装置的残障用户的访问体验。

对象：使用<a href="http://baike.baidu.com/view/2594250.htm" target="_blank">屏幕阅读器</a>的这类残障用户。

WIA-ARIA（富英特网应用的可访问性）规范提供了若干方法来提高网站尤其是web应用的可访问性，很多屏幕阅读软件已经开始使用WIA-ARIA规范到的内容了。下面讲的这些技术不需要事先兼容性支持，因为很多的屏幕阅读软件都已经采用了这些技术。

其实主要就是利用html5的新属性role .有两种特有的角色可以现在就利用这个属性：标志角色(landmark)和文档角色(document).

一：标志角色

标志角色用来标识网站上有意义的区域，如banner,搜索区域，或导航等等可以快速被屏幕阅读器识别的元素

banner       识别页面的banner区域

search    识别页面中的搜索区域

navigation    识别页面中的导航元素

main  识别页面中主要内容的起始处

contentinfo     识别内容的相关信息的位置

complementary  识别一块包含web应用的页面区域，而非web文档。

例如在页面的总头部中，加入banner角色：
<blockquote>&lt;header id="page-header"  role="banner"&gt;

&lt;h1&gt;Awesomeco Blog&lt;/h1&gt;

&lt;/header&gt;</blockquote>
在主题区域和侧边栏可以定义如下：
<blockquote>&lt;section id="posts" role="main"&gt;

&lt;/section&gt;   //main

&lt;section id="sidebar" role="complementary"&gt;

&lt;nav&gt;

&lt;ul&gt;

&lt;li&gt;&lt;/li&gt;

&lt;li&gt;&lt;/li&gt;

&lt;li&gt;&lt;/li&gt;

&lt;/ul&gt;

&lt;/nav&gt;

&lt;/section&gt;//sidebar</blockquote>
二：文档结构角色（document）

借助文档结构角色可以让屏幕阅读器轻松识别一些固定的内容，以便建立导航的页面布局

document  识别文档内容区域，相对于应用内容区域来说

article  识别组成文档独立内容的段落

definition   识别对短语或主题的定义

directory  识别对于一个集合元素的引用列表，比如一个内容表格。

heading   标识一个页面的头部

img     标识一段包含图片的区域。该区域包含图片或者标题和和说明文字

list  标识一组不可交互的列表项

listitem   标识一组或一个不可交互的列表项

math   标识一个数学表达式

note 标识对主要内容的注释或补充

presentation   标识可以被辅助装置忽略的用于美化外观的内容

row  标识表格中的一行

rowheader  标识表格中包含字段含义的表格头

例如我们可以在博客的body元素添加document 角色
<blockquote>&lt;body role="document"&gt;</blockquote>
-----------------------完，欢迎大家往右边看，关注我的微信号




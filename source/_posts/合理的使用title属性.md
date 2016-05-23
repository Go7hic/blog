---
layout: post
title: 合理的使用title属性
date: 2014-06-03 11:21:04
tags: html5
---

####title属性不友好用户如下

- 手机用户
- 仅使用键盘的用户
- 使用屏幕放大器的用户
- 屏幕阅读器用户
- 精细运动技能障碍的用户
- 认知障碍的用户

####title属性有用的例子：

**为frame或iframe元素贴上标签：**
- <frame title="navigation">
**提供需要程序才能实现的在特殊情况下才显示的标签，直接使用可见的文本标签会显得多余：**
- <input type="text" title="search"> <input type="submit" value="search">
- 数据表格中的标签控件。 ###title属性无用或用处不大的例子：
**为不能作为文本的链接或周围内容添加额外信息：**
- <a href="newsletter.PDF" title="PDF file, size 1 mb.">newsletter</a>
- 相反这样的信息应该作为链接文本的部分或在链接的旁边。
**提供和链接文本相同的信息：**
- <a href="newsletter.PDF" title="newsletter">newsletter</a>
- 建议不要复制链接内容作为title属性。这其实相当于什么都没做。
**用于图像的标题：**
- <img src="castle1858.jpeg" title="Oil-based paint on canvas. Maria Towle, 1858." alt="The castle now has two towers and two walls.">
- 大概标题信息是最重要的信息，应该能被所有用户默认访问。如果是这样，那么这个内容应该紧挨着图片。
**用来代替表单的标签，去掉可见的文本标签：**
- <input type="text" title="name">
- 屏幕阅读器的用户将会访问表单元素的标签，由于title属性被列入可访问性api内的属性名称（当文本标签使用标签元素时是不被支持的）。许多其他用户并不如此。建议尽可能包括一个可见的文本标签。
**为表单元素提供和可见的标签内容相同的信息：**
- <label for="n1">name</label> <input type="text" title="name" id="n1">
- 重复可见的标签文本不可能除了添加一系列的用户认知噪声。不做它。重复可见的标签文本除了添加一系列令人讨厌的认知噪声外，似乎没有其他作用，停止这种用法。
**为表单元素提供额外的指令：**
- <label for="n1">name</label> <input type="text" title="Please use uppercase."id="n1">
- 如果这指令对于正确的使用表单元素非常重要，请在元素周围提供文字信息，确保每个用户都能读到。
**作为缩写的扩展：**
- <abbr title="world wide web consortium">W3C</abbr>
- 虽然abbr元素的title属性被屏幕阅读器软件所支持，但使用它仍然是有问题的，因为其他用户群无法使用。建议当缩写词在文档中首次出现时提供文本格式的全称，或提供全称形式的术语表。这并不是说不可以使用title属性，因其具有局限性，应该提供文本形式的全称。

####HTML 5.1 包括使用title属性的一般性建议：

依赖title属性目前是不被鼓励的，由于许多用户代理不能按照规范的要求显示这个属性（如需要鼠标指针设备引起提示信息的显示，排除了仅使用键盘的用户和触摸屏用户）
来源: HTML 5.1 – title属性

用title属性代替img元素的alt属性或作为图片的标题是被禁止的

依托title属性目前来看是被禁止的，由于许多用户代理对这属性的可访问性支持很差……
来源: HTML 5.1




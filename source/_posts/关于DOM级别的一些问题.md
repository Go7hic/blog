---
layout: post
title: 关于DOM级别的一些问题
date: 2013-12-20 11:21:04
tags: JavaScript
---

之前看书没太注意这个问题，直到我今天看书看到一个DOM0级，于是我就在群里问了下各个级别的意思区别..

首先我们的确定标准了是没有DOM0级的。在平时阅读的时候可能会读到DOM0级(DOM Level0)的字眼。实际上，DOM0级标准是不存在的，所谓的DOM0级是DOM历史坐标中的一个参照点而已，具体说呢，DOM0级指的是IE4和Netscape 4.0这些浏览器最初支持的DHTML..大概2000年的时候争论过DOM0的问题，最后结论大概是，没有官方形成此标准.。

DOM1级（DOM Level 1）于1998年10月成为W3C的推荐标准。DOM1级由两个模块组成：DOM核心（DOM Core）和DOM HTML。其中，DOM核心规定的是如何映射基于XML的文档结构，以便简化对文档中任意部分的访问和操作。DOM HTML模块则在DOM核心的基础上加以扩展，添加了针对HTML的对象和方法。

如果说DOM1级的目标主要是映射文档的结构，那么DOM2级的目标就要宽泛多了。DOM2级在原来DOM的基础上又扩充了（DHTML一直都支持的）鼠标和用户界面事件、范围、遍历（迭代DOM文档的方法）等细分模块，而且通过对象接口增加了对CSS（Cascading Style Sheets，层叠样式表）的支持。DOM1级中的DOM核心模块也经过扩展开始支持XML命名空间。

DOM2级引入了下列新模块，也给出了众多新类型和新接口的定义。

DOM视图（DOM Views）：定义了跟踪不同文档（例如，应用CSS之前和之后的文档）视图的接口；

DOM事件（DOM Events）：定义了事件和事件处理的接口；

DOM样式（DOM Style）：定义了基于CSS为元素应用样式的接口；

DOM遍历和范围（DOM Traversal and Range）：定义了遍历和操作文档树的接口。

DOM3级则进一步扩展了DOM，引入了以统一方式加载和保存文档的方法–在DOM加载和保存（DOM Load and Save）模块中定义；新增了验证文档的方法–在DOM验证（DOM Validation）模块中定义。DOM3级也对DOM核心进行了扩展，开始支持XML 1.0规范，涉及XML Infoset、XPath和XML Base。

在阅读DOM标准的时候，读者可能会看到DOM0级（DOM Level 0）的字眼。实际上，DOM0级标准是不存在的；所谓DOM0级只是DOM历史坐标中的一个参照点而已。具体说来，DOM0级指的是Internet Explorer 4.0和Netscape Navigator 4.0最初支持的DHTML。

####其他DOM标准

除了DOM核心和DOM HTML接口之外，另外几种语言还发布了只针对自己的DOM标准。下面列出的语言都是基于XML的，每种语言的DOM标准都添加了与特定语言相关的新方法和新接口：

SVG（Scalable Vector Graphic，可伸缩矢量图）1.0；

MathML（Mathematical Markup Language，数学标记语言）1.0；

SMIL（Synchronized Multimedia Integration Language，同步多媒体集成语言）。

还有一些语言也开发了自己的DOM实现，例如Mozilla的XUL（XML User Interface Language，XML用户界面语言）。但是，只有上面列出的几种语言是W3C的推荐标准。

话说最新的w3c草案里还有一个DOM4……….




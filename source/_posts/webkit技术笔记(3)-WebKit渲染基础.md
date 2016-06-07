---
layout: post
title: webkit技术笔记(3):WebKit渲染基础
date: 2014-09-18 11:21:04
tags: webkit
---
上篇有两个词语RenderObjec，RenderLayer么看书的同学可能不清楚，我就直接把网上的说明摘下来了了。
WebKit是一个渲染引擎，而不是一个浏览器，它专注于网页内容展示，其中渲染是其中核心的部分之一。本章着重于对渲染部分的基础进行一定程度的了解和认识，主要理解基于DOM树来介绍Render树和RenderLayer树的构建由来和方式。

那么什么是DOM？简单来说，DOM是对HTML或者XML等文档的一种结构化表示方法，通过这种方式，用户可以通过提供标准的接口来访问HTML页面中的任何元素的相关属性，并可对DOM进行相应的添加、删除和更新操作等。相关信息可查阅W3C的文档，这里不再赘述。

基于DOM树的一些可视（visual）的节点，WebKit来根据需要来创建相应的RenderObject节点，这些节点也构成了一颗树，称之为Render树。基于Render树，WebKit也会根据需要来为它们中的某些节点创建新的RenderLayer节点，从而形成一棵RenderLayer树。

Render树和RenderLayer树是WebKit支持渲染所提供的基础但是却非常重要的设施。这是因为WebKit的布局计算依赖它们，浏览器的渲染和GPU硬件加速也都依赖于它们。幸运地是，得益于它们接口定义的灵活性，不同的浏览器可以很方便地来实现自己的渲染和加速机制。

为了直观了解这三种树，下图给出了这三种树及其它们之间的对应关系，后面会详细介绍里面的细节。

![](http://dyy.im/wp-content/uploads/2014/09/01YPYqU2exxr.jpg)


Render树

Render树的建立

Render树是基于DOM树建立起来的一颗新的树， 是布局和渲染等机制的基础设施。 Render树节点和DOM树节点不是一一对应关系，那么哪些情况下需要建立新的Render节点呢？

a) DOM树的document节点；

b) DOM树中的可视化节点，例如HTML，BODY，DIV等，非可视化节点不会建立Render树节点，例如HEAD，META，SCRIPT等；

c) 某些情况下需要建立匿名的Render节点，该节点不对应于DOM树中的任何节点；

RenderObject对象在DOM树创建的同时也会被创建，当然，如果DOM中有动态加入元素时，也可能会相应地创建RenderObject对象。下图示例的是RenderObject对象被创建的函数调用过程。

![](http://dyy.im/wp-content/uploads/2014/09/01YPYqV6vhGu.jpg)

Render树建立之后，布局运算会计算出相关的属性，这其中有位置，大小，是否浮动等。有了这些信息之后，渲染引擎才只知道在何处以及如何画这些元素。

RenderObject类及其子类

RenderObject是Render树的节点基础类，提供了一组公共的接口。它有很多的子类，这些子类可能对应一些DOM树中的节点，例如RenderText，有些则是容器类，例如RenderBlock。下图给出了一些常用的类的继承关系图，这其中RenderBlock是一个非常重要的类。 
![](http://dyy.im/wp-content/uploads/2014/09/01YPYqVHOOaQ.jpg)

匿名RenderBlock对象

CSS中有块级元素和内嵌(inline)元素之分。内嵌元素表现的是行布局形式，就是说这些元素以行进行显示。以’div’元素为例，如果设置属性’style’为’display:inline’时，则那是内嵌元素，那么它可能与前面的元素在同一行；如果该元素没有设置这个属性时，则是块级元素，那么在新的行里显示。 RenderBlock用来是用来表示块级元素， 为了处理上的方便，某些情况下需要建立匿名的RenderBlock对象，因为RenderBlock的子女必须都是内嵌的元素或者都是非内嵌的元素。所以，当它包含两种元素的时候，那么它会为相邻的内嵌元素创建一个块级RenderBlock节点，然后设置该节点为自己的子女并且设置这些内嵌元素为它的子女。

RenderLayer树

RenderLayer树是基于Render树建立起来的一颗新的树。同样，RenderLayer节点和Render节点不是一一对应关系，而是一对多的关系。那么哪些情况下的RenderObject节点需要建立新的RenderLayer节点呢？

a) DOM树的document节点对应的RenderView节点

b) DOM树中的document 的子女节点，也就是HTML节点对应的RenderBlock节点

c) 显式的CSS位置

d) 有透明效果的对象

e) 节点有溢出（overflow），alpha或者反射等效果的

f) Canvas 2D和3D (WebGL)

g) Video节点对应的RenderObject对象

一个RenderLayer被建立之后，其所在的RenderObject对象的所有后代均包含在该RenderLayer，除非这些后代需要建立自己的RenderLayer。而后代的RenderLayer的父亲就是自己最近的祖先所在的不同的RenderLayer，这样，它们也构成了一颗RenderLayer树。

每个RenderLayer对应的Render节点内容均会绘制在该RenderLayer所对应的层次上（或者内部存储结构上）。不同的RenderLayer可以共享同一个内部存储结构，也可以有各自的后端存储，这取决于不同的移植。在软件渲染下，通常各个RenderLayer的内容都绘制在同一块后端存储上。在GPU硬件加速的下，某些RenderLayer可能有自己独立的后端存储，而后通过合成器来把这些不同的后端合成在一起，最终形成网页的可视化内容。

RenderLayer在创建RenderObject对象的时候，如果需要，也会同时被创建，当然也有可能在执行JavaScript代码时，更新页面的样式从而需要新创建一个RenderLayer。

## 一个例子

以上说了那么多，一堆堆的类，一个个的建立规则，听起来太抽象，不太容易理解，那么一个具体的Render树和RenderLayer树到底是怎么样的呢？为了可视化理解Render树和RenderLayer树，下面给出一个具体的例子来加以解释和说明。 首先，让我们来看一个简单的HTML网页，源代码如下所示。
![](http://dyy.im/wp-content/uploads/2014/09/01YPYqVeqpKW.jpg)


上面的代码结构很简单，就是一些HTML的基本元素组成的，例如HTML，HEAD，DIV，A, IMG， TABLE等等， 然后包含一个特别的HTML5元素CANVAS，而且还有一小段JavaScript代码。照顾到一些没有相关背景的读者，简单解释一下这段代码的含义。这段代码是对CANVAS元素创建一个WebGL的Context，有了这个context，就可以在canvas元素上绘制3D的内容。这个类似于OpenGL或者OpenGLES的context，具体canvas和WebGL会有另外专门的章节来介绍。

这段HTML源代码被WebKit解析后会生成一颗DOM树。这段代码的DOM树主要结构如本章第一幅图中的‘DOM树’部分所示。当DOM树生成时，WebKit同时建立了一颗Render树，如上所说，代码的Render树被打印成如下图所示的文本信息。

![](http://dyy.im/wp-content/uploads/2014/09/01YPYqXJZATr.jpg)

首先，上图中的“layer at (x, x)”表示不同的RenderLayer节点，下面的所有RenderObject均属于该RenderLayer。以第一个layer为例，它对应于DOM中的Document节点。后面的(0, 0)表示该节点在坐标系中的位置，最后的1028x683表示该节点的大小。它包含的RenderView节点后面的信息也是同样的意思。

其次，看其中最大的部分－第二个layer，包含了HTML中的绝大部分元素。这里有三点需要解释一下：第一，Head元素没有相应的RenderObject，如上面所解释的，因为它不是一个可视的元素。第二，Canvas元素不在其中，而是在第三个layer中（RenderHTMLCanvas）。但是它仍然是RenderBody节点的子女。第三，匿名的RenderBlock节点，它包含了RenderText， RenderInline等节点，原因如前所说。

再次，来看一下第三个layer。因为从Canvas创建了一个WebGL3D Context对象，这里需要重新生成一个新的layer。

最后，来说明一下三个layer的创建时间。第一和第二个layer在创建DOM树后，会触发创建；第三个layer测试资源加载解析好之后，执行后面的JavaScript代码后所创建。

基于上面的描述，相信大家已经对Render树和RenderLayer树有了一定的了解。




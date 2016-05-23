---
layout: post
title: CSS之BFC详解
date: 2014-03-06 11:21:04
tags: css
---


最近总结了一个前端学习的方法，就是3W(What,Why,hoW)方法。其实对于很多前端知识点还有一些api都可以这样去学习，当然这也是从平常的应试教育学习中借鉴过来的，接下来我们用这个方法来学一下BFC这个东西。

What:了解该知识点的概念，本质以及有关牵扯到的相关知识概念

BFC这个东西说常见的话你可能不觉得，但是你肯定会常用，也许你在用的时候也没想到BFC这东西。网上也有很多写这些东西的文章，但是自己写一遍印象更深一点。

首先我们看看w3c对BFC是怎么定义的:

http://www.w3.org/TR/CSS2/visuren.html#block-formatting

>Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with ‘overflow’ other than ‘visible’ (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents. In a block formatting context, boxes are laid out one after the other, vertically, beginning at the top of a containing block. The vertical distance between two sibling boxes is determined by the ‘margin’ properties. Vertical margins between adjacent block-level boxes in a block formatting context collapse. In a block formatting context, each box’s left outer edge touches the left edge of the containing block (for right-to-left formatting, right edges touch). This is true even in the presence of floats (although a box’s line boxes may shrink due to the floats), unless the box establishes a new block formatting context (in which case the box itself may become narrower due to the floats).

用谷歌翻译(笑..我是谷歌脑残粉)过来就是：

>浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的块级格式化上下文。 在一个块级格式化上下文里，盒子从包含块的顶端开始垂直地一个接一个地排列，两个盒子之间的垂直的间隙是由他们的margin 值所决定的。两个相邻的块级盒子的垂直外边距会发生叠加。 在块级格式化上下文中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘），即使存在浮动也是如此，除非这个盒子创建一个新的块级格式化上下文。

从上面我们还可找出几个几个比较重要的概念东西，比如:boxe , block  formatting context。毫无疑问BFC就是block  formatting context的缩写，中文就是“块级格式化上下文”的意思。我们在那个w3c那个页面发现还有其他inline formatting context，所以我们可以看看 formatting context是个什么东西：

Formatting context是W3C CSS2.1规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

最常见的Formatting context有Block fomatting context(简称BFC)和Inline formatting context(简称IFC)。CSS2.1 中只有BFC和IFC, CSS3中还增加了GFC和FFC.

至于那个box还要讲吗？嗯，还是回顾一下：

Box是CSS布局的对象和基本单位， 直观点来说，就是一个页面是由很多个Box(即boxes)组成的。元素的类型和display属性，决定了这个Box的类型。 不同类型的Box， 会参与不同的Formatting context(一个决定如何渲染文档的容器)，因此Box内的元素会以不同的方式渲染。常见的盒子类型

block-level box: display属性为block, list-item, table的元素，会生成block-level box。并且参与block fomatting context。
inline-level box: display属性为inline, inline-block, inline-table的元素，会生成inline-level box。并且参与inline formatting context。

妈蛋四级刚刚飘过的孩子看这点英文不容易啊，有时候我们总觉的书上或者官方的概念定义的东西不利于我们理解，所以我们更喜欢有些老师通俗的讲解。这里我们也通俗的理解一下：

BFC就是“块级格式化上下文”的意思，创建了 BFC的元素就是一个独立的盒子，不过只有Block-level box可以参与创建BFC， 它规定了内部的Block-level Box如何布局，并且与这个独立盒子里的布局不受外部影响，当然它也不会影响到外面的元素。

BFC有一下特性：

内部的Box会在垂直方向，从顶部开始一个接一个地放置。
Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生叠加
每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
BFC的区域不会与float box叠加。
BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。
计算BFC的高度时，浮动元素也参与计算。
好了What这一阶段就到这了，基本的概念我们都要了解清楚，不清楚的多看几遍，是在不清楚的我猜是我写的不够通俗易解。

接下来我一般会考虑Why，即为什么会出现这个问题，为什么要这样用，为什么会出现这些效果。但是这里就不写了，因为我也不知道写啥(哭…求高手指点)…

到最后我就是考虑How了，不用说你也知道了，就是怎么解决这些问题，这些知识点该怎么用，还有没有其他的方法..

那么我们该怎么使用BFC呢，如何触发BFC呢？：
```
float 除了none以外的值
overflow 除了visible 以外的值（hidden，auto，scroll ）
display (table-cell，table-caption，inline-block, flex, inline-flex)
position值为（absolute，fixed）
fieldset元素
```
在以上的情况里可以创建BFC。

接下我们看下怎么运用BFC，在哪些场景可以用到BFC.

1.解决margin叠加问题

HTML :
```
<p>
    hello world
</p>
<p>
    hello world
</p>
<p>
    hello world
</p>
```

CSS :
```
p {
        color:black;
        background: #FF0000;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 50px;
    }
```
三P每个p之间的距离为50px，发生了外边距叠加。 要解决这个叠加问题即让每个P之间是100px，我们可以新建一个BFC，怎么建呢？可以给p元素添加一个父元素，让它触发BFC。如下:

HTML :
```
<div class="BFC">
    <p>
    hello world
    </p>
</div>
 
<p>
    hello world
</p>
<p>
    hello world
</p>
```
CSS :
```
.BFC{
    overflow:hidden;
}
p{
    color:black;
    background:#F00;
    line-height:100px;
    width:200px;
    text-align:center;
    margin:50px;
}
    
```

2.用于布局

HTML :
```
<div class="aside"></div> 
<div class="main"></div> 
```
CSS :
```
body {
        width: 300px;
        position: relative;
    }
.aside {
        width: 100px;
        height: 150px;
        float: left;
        background: blue;
     
}
     
 .main {
        height: 200px;
                background: #f00;
}
```
从图中我们会发现上面BFC的第三个特性，就是元素的左外边距会触碰到包含块容器的做外边框，就算存在浮动也会如此。那么我们如何解决这个问题呢？看上面BFC第四个特性，就是BFC不会与浮动盒子叠加，那么我们是不是可以创建一个新的BFC来解决这个问题呢？来看看：

HTML :
```
<div class="aside"></div> 
<div class="main"></div>
```
CSS :
```
body {
        width: 300px;
        position: relative;
    }
.aside {
        width: 100px;
        height: 150px;
        float: left;
        background: blue;
     
}
     
 .main {
        height: 200px;
                background: #f00;
                overflow:hidden;
}
```

发现我们用overflow:hidden触发main元素的BFC之后，效果立马出现了,一个两栏布局就这么妥妥的搞掂…
3.用于清除浮动，计算BFC高度.

因为上面第六个特性提到计算BFC高度时，浮动元素也会参与计算，我们先看一个例子：

BFC5 : 在线演示查看源码
HTML :
```
<div class="BFC">
    <div class="box"></div>
    <div class="box"></div>
</div>
```
CSS :
```
.BFC {
        border: 5px solid #f00;
        width: 300px;
    }
 
    .box {
        border: 5px solid blue;
        width:100px;
        height: 100px;
        float: left;
    }
```
我们发现由于里面两个子元素浮动的关系，两个box已经脱离了父元素的包含块，父元素高度已经塌陷，我们需要让父元素包含两个box子元素，这样计算高度时，两个浮动子元素就会参与，所以我们要闭合浮动，触发父元素的BFC，我们还是继续用overflow:hidden来看看效果吧：
BFC6 : 在线演示查看源码
HTML :
```
<div class="BFC">
    <div class="box">
         
    </div>
    <div class="box">
         
    </div>
</div>
```
CSS :
```
.BFC {
        border: 5px solid #f00;
        width: 300px;
          overflow:hidden;
    }
 
    .box {
        border: 5px solid blue;
        width:100px;
        height: 100px;
        float: left;
    }
```

怎么样，效果还很明显的吗，当然清理(闭合)浮动还有很多方法，大家可以看看一丝大神写的那些年我们一起清除过的浮动。
好了写到这里基本才不多了，BFC是个很奇怪的东西，她一直隐式的存在我们的css样式里，但是我们要记住BFC是页面元素里一个独立存在作用块，它不影响它外面的布局，外面的元素也不会影响到BFC里面的布局。

参考文章：http://www.w3.org/TR/CSS2/visuren.html#block-formatting,http://www.iyunlu.com/view/css-xhtml/55.html，http://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html




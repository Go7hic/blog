---
layout: post
title: css之伪类和伪元素区别详解
date: 2014-01-24 11:21:04
tags: CSS
---

伪类和伪元素是个古老的话题，网上也很多教程把两者混为一谈。今天看到<精通css>一书里好像把`:after`说成伪类，所以我觉得有必要在回顾一下这个知识点。

无论是伪类还是伪元素，都属于CSS选择器的范畴。所以它们的定义可以在CSS标准的选择器章节找到。分别是 CSS2.1 Selectors 和 CSS Selector Level 3，两者都已经是推荐标准。

####标准的定义

在CSS2.1里，5.10 Pseudo-elements and pseudo-classes 描述了这两个概念的由来，它们是被一同提及的。但到了 Selector Level 3 里，它们就被分开到两个小节里加以区分。但无论如何，伪类和伪元素的引入都是因为在文档树里有些信息无法被充分描述，比如CSS没有“段落的第一行”之类的选择器，而这在一些出版场景里又是必须的。用标准里的话说：

>CSS introduces the concepts of pseudo-elements and pseudo-classes to permit formatting based on information that lies outside the document tree.

简单翻译一下，就是：

>CSS 引入伪类和伪元素的概念是为了实现基于文档树之外的信息的格式化
这么说很抽象，其实就是为了描述一些现有CSS无法描述的东西。缺少什么，则引入什么，不管是标准，还是人，都是如此成长而来。

####伪类与伪元素的区别

这里我大可以列一个表格，把所有的伪类和伪元素分开罗列，但这未免太形式化，与其记住“哪些是哪些不是”，不如真正地加以区分。伪类和伪元素本身就有一个根本的不同之处，这点直接体现在了标准的描述语句上。

先看一个伪元素 first-line 例子。现在有一段HTML，内容是一个段落：
```
<p>I am the bone of my sword. Steel is my body, and fire is my blood.  
I have created over a thoustand blades.  
Unknown to Death.Nor known to Life. Have withstood pain to create many weapon.  
Yet, those hands will never hold anything. So as I pray, unlimited blade works.</p>  
```
如果我要描述这个段落的第一行，在不用伪元素的情况下，我会怎么做？想来我一定要嵌套一层 span，然后加上类名:
```
<p><span>I am the bone of my sword. Steel is my body, and fire is my blood. </span>  
I have created over a thoustand blades.  
Unknown to Death.Nor known to Life. Have withstood pain to create many weapon.  
Yet, those hands will never hold anything. So as I pray, unlimited blade works.</p>  
```
再反观一个伪类 first-child 的例子，有一个简单的列表：
```
<ul>  
    <li></li>
    <li></li>
</ul> 
```
如果我要描述 ul 的第一个元素，我无须嵌套新的元素，我只须给第一个已经存在的 li 添加一个类名就可以了：
```
<ul>  
    <li></li>
    <li></li>
</ul> 
```
尽管，第一行和第一个元素，这两者的语意相似，但最后作用的效果却完全不同。所以，伪类和伪元素的根本区别在于：它们是否创造了新的元素(抽象)。从我们模仿其意义的角度来看，如果需要添加新元素加以标识的，就是伪元素，反之，如果只需要在既有元素上添加类别的，就是伪类。而这也是为什么，标准精确地使用 “create” 一词来解释伪元素，而使用 “classify” 一词来解释伪类的原因。一个描述的是新创建出来的“幽灵”元素，另一个则是描述已经存在的符合“幽灵”类别的元素。

伪类一开始单单只是用来表示一些元素的动态状态，典型的就是链接的各个状态(LVHA)。随后CSS2标准扩展了其概念范围，使其成为了所有逻辑上存在但在文档树中却无须标识的“幽灵”分类。

伪元素则代表了某个元素的子元素，这个子元素虽然在逻辑上存在，但却并不实际存在于文档树中。

伪类和伪元素混淆的由来

最为混淆的，可能是大部分人都将 `:before` 和 `:after` 这样的伪元素随口叫做伪类，而且即使在概念混淆的情况下，实际使用上也毫无问题——因为即使概念混淆，对真正使用也不会造成多少麻烦:)

CSS Selector Level 3 为了区分这两者的混淆，而特意用冒号加以区分：

伪类用一个冒号表示 `:first-child`
伪元素则使用两个冒号表示 `::first-line`
并且规定，浏览器既要兼容CSS1和2里既存的伪元素的单冒号表示，同时又要不兼容CSS3新引入的伪元素的单冒号表示。后来的结果大家都知道，因为低版本IE对双冒号的兼容问题，几乎所有的CSSer在写样式的时候都不约而同的使用了单冒号。这无形中，让这种混淆延续了下来。而CSS3新伪元素的使用到目前为止，还远远不成气候。

伪类和伪元素常见的应用场景

列表处理

伪类与伪元素很大部分常规用途是在处理列表的。特别是 `:nth-child()` 这类结构伪类。

页脚链接

页脚链接可能是一个最为合适的例子，因为例子里伪元素和伪类并用。很多网站的页脚链接是用竖线分隔的罗列<a>标签形式。在这种页脚链接里使用伪类和伪元素的组合，就可以让链接的列表的HTML代码变得非常干净整洁：
```
ul.list{list-style:none;}  
ul.list li{float:left;}  
ul.list li:after{content:"\00a0|\00a0";}  
ul.list li:last-child::after{content:"";}  
```
但当前，放眼望去几乎不会有人这么做。绝大部分依然使用旧的方式。除了一些维护上的考量，习惯也是一个主要原因。熟优熟略这里不讨论，这里只是作为最典型的例子展示它们的应用。

最后项，奇偶项，倍数项

另一个广为人知的例子是：去掉列表最后一个元素的边框或者边距。在这种情况下，我们似乎已经习惯了给最后一个元素加上一个类似last-item的类名，然后单独写一条CSS规则。
```
ul.list li{margin-right:10px;}  
ul.list li.last-item{margin-right:0;}  
```
实际上，我自己在写的时候也会偏向一个早就习惯了的做法。在时间限定的情况下，不会考虑激进的方案，也不会考虑类似负边距或者超界显示等等优化方案，而只是一味的寻求最为保险和熟悉的习惯上的东西。习惯是可怕的顽固，所以可能今后还是会有很长一段时间不会写成下面这样：
```
#ul.list li{margin-right:10px;}
#ul.list li.last-child{margin-right:0;}
```
隔行背景色可能是奇偶项使用最多的情况，但现在又有多少人已经完全使用odd和even来替代额外添加的类名？
```
tr:nth-child(odd){background-color:#f4f4f4;}  
tr:nth-child(even){background-color:#f9f9f9;} 
```
还有诸如 Gallery 的特定项的边距，伪类的处理也不错不是嘛？
```
li:nth-child(3n) {margin-right:0;}  
```
不过眼下，似乎更多人用负边距或者inline-block来处理，甚至直接让透明边距顶出容器，优化的手法已经很多，而且在维护性上甚至更有优势。

彩色

这其实还是上面的命题，但却单独拿出来说。如今的网页越来越多彩，也就不免单独指定一些亮丽的颜色。还是有很多人将特定的颜色用特定的类名标注。然而，如果颜色的映射顺序不怎么重要的话的，也许只是要达到多彩的简单目的，完全可以这么做：
```
li:nth-child(1) a{color:#f30;}  
li:nth-child(2) a{color:#c0f;}  
li:nth-child(3) a{color:#90f;}  
li:nth-child(4) a{color:#06f;}  
li:nth-child(5) a{color:#0cf;}  
```
应对各种区块的色系调整，伪类的使用可以大幅减少HTML里的单一用途的类名。顺着这个例子推荐一个网站：xycss.com。这是一个由 Jeff Starr 建立的关于响应栅格布局的网站，他撰写的框架 xy.css 也有非常多的可以学习的地方。当然在这里举其作为例子还是因为这个网站的首页和这个例子相符——伪类实现的多彩的色块。

####清除浮动

这是一个被讲烂的命题，不过我必须将其列在伪元素的典型应用的列表里，因为一般而言，伪元素(:after)在这里总是最为常见。

####非典型应用

所谓的非典型应用其实无非就是“不干正经事”的意思，与当年的table多少有些神似。这部分里有一些已经淘汰的手法，但为了纪念它们存在的痕迹，还是将它们例举和提及。

####阴影

如今的浏览器大部分都已经支持box-shadow这个方便的阴影制造器，也正因为此，box-shadow在如今的使用中颇为频繁。关于box-shadow的文章我也至少写过4篇：

虽然如今愈发觉得这个属性存在被滥用的危险，但是实际使用时，制造阴影往往需要和伪元素一起使用，特别是当页面中的某些布局需要额外元素制造阴影的时候。由于凡是支持box-shadow的浏览器都支持伪元素，所以几乎任何时候要添加box-shadow，伪元素都是考虑范围之一。比如，给全局网页加上一个顶部的阴影渐变：
```
body:before{content:"";position:fixed;top:-15px;left:0;width:100%;height:15px;box-shadow:0px 0px 15px rgba(150,150,150,.8);}  
```
####CSS调试

有关 :empty 伪类的应用。
```
div:empty,  
span:empty,  
li:empty,  
p:empty,  
td:empty,  
th:empty{padding:1em;background-color:#ff0 !important;} 
```
:empty可以在开发过程中以可视化的方式过滤空标签。

####额外的提示

比如给相应的链接增加相应的内容提示:
```
.example-block:before {
   content: "Example:";
}
```
在我的印象里，早期的W3c标准文档里就是类似这么干的。这种额外信息的提示源自伪元素和CSS Content的配合使用，更多例子推荐阅读 Chris Coyier写的CSS Content

####内描边和背景透明

这两个算是浏览器在CSS2的大时代背景里夹缝里生存的trick，如今看来早已经过时。现在的内描边可以用box-shadow轻而易举的完成，透明背景则需要用到CSS的RGBA以及IE滤镜。

参考：http://swordair.com/typical-and-atypical-usage-of-css-pseudo-classes-and-pseudo-elements/




---
layout: post
title: css布局之三栏布局
date: 2014-02-13 11:21:04
tags: CSS
---
这里我回顾两种情况，分别是中间栏自适应宽度，两边固定宽度和中间栏固定宽度，两边栏自适应宽度。

###第一种情况：中间栏自适应宽度，两边固定宽度
常用的方法还是那三种绝对定位，浮动定位，负边距方法

1.绝对定位:左右两栏采用绝对定位，分别固定于页面的左右两侧，中间的主体栏用左右margin值撑开距离
```
<div id=”left”></div>
<div id=”main”></div>
<div id=”right”></div>
```
css
```
html,body{margin:0; height:100%;}
#left,#right{position:absolute; top:0; width:200px; height:100%;}
#left{left:0; background:#a0b3d6;}
#right{right:0; background:#a0b3d6;}
#main{margin:0 210px; background:#ffe6b8; height:100%;}
```
ps:这里的左中右三个div的顺序是可以任意调整的.此方法的优点是，理解容易，上手简单，受内部元素影响而破坏布局的概率低，就是比较经得起折腾。
缺点在于：如果中间栏含有最小宽度限制，或是含有宽度的内部元素，当浏览器宽度小到一定程度，会发生层重叠的情况。

2.浮动方法：左栏左浮动，右栏右浮动，主体直接放后面，就实现了自适应。
```
<div id=”left”></div>
<div id=”right”></div>
<div id=”main”></div>
```
css
```
html,body{margin:0; height:100%;}
#main{height:100%; margin:0 210px; background:#ffe6b8;}
#left,#right{width:200px; height:100%; background:#a0b3d6;}
#left{float:left;}
#right{float:right;}
```
ps:这里三个div标签的顺序的关键是要把主体div放在最后，左右两栏div顺序任意。
此方法的优点是：代码足够简洁与高效
不足在于：中间主体存在克星，clear:both属性。如果要使用此方法，需避免明显的clear样式

3:负边距方法：这个方法有点复杂，但是用的估计是最多的。方法很难用一句话概括。首先，中间的主体要使用双层标签。外层div宽度100%显示，并且浮动（本例左浮动，下面所述依次为基础），内层div为真正的主体内容，含有左右210像素的margin值。左栏与右栏都是采用margin负值定位的，左栏左浮动，margin-left为-100%，由于前面的div宽度100%与浏览器，所以这里的-100%margin值正好使左栏div定位到了页面的左侧；右侧栏也是左浮动，其margin-left也是负值，大小为其本身的宽度即200像素。
```
<div id=”main”>
<div id=”content”></div>
</div>
<div id=”left”></div>
<div id=”right”></div>
```
css
```
html,body{margin:0; height:100%;}
#main{width:100%; height:100%; float:left;}
#main #body{margin:0 210px; background:#ffe6b8; height:100%;}
#left,#right{width:200px; height:100%; float:left; background:#a0b3d6;}
#left{margin-left:-100%;}
#right{margin-left:-200px;}
```
ps:您需要注意几个div的顺序，无论是左浮动还是右浮动，先是主体部分div，这是肯定的，至于左右两栏谁先谁后，都无所谓.此方法的优点：三栏相互关联，可谓真正意义上的自适应，有一定的抗性——布局不易受内部影响。
缺点在于：相对比较难理解些，上手不容易，代码相对复杂。出现百分比宽度，过多的负值定位，如果出现布局的bug，排查不易。

###二.中间固定宽度，两边自适应宽度
1.负边距方法
```
<div id=”left”>
<div class=”inner”>this is left sidebar content</div>
</div>
<div id=”main”>
<div class=”inner”>this is main content</div>
</div>
<div id=”right”>
<div class=”inner”>this is right siderbar content</div>
</div>
```
css:
```
#left,
#right {
float: left;
margin: 0 0 0 -271px;
width: 50%;
*width: 49.9%;
}
#main {
width: 540px;
float: left;
background: green;
} .inner {
padding: 20px;
} #left .inner,
#right .inner {
margin: 0 0 0 271px;
background: orange;
}
```
这种方法也是借助于负的margin来实现的，首先我们在中间列定好固定值，因为此值是不会在改变的，接着对其进行左浮动；那么关键地主是在左右边栏设置地方，这种方法是将其都进行50%的宽度设置，并加上中负的左边距，此负的左边距最理想的值是中间栏宽度的一半加上1px，比如说此例中是”540px/2+1″也就是说他们都有一个”margin-left: -271px”,这样一来，左右边栏内容无法正常显示，那是因为对他们进行了负的左边距操作，现在只需要在左右边栏的内层div.inner将其拉回来，就ＯＫ了

2.CSS3 Flexbox
```
<div class=”grid”>
<div class=”col fluid”>
…
</div>
<div class=”col fixed”>
…
</div>
<div class=”col fluid”>
…
</div>
</div>
```
CSS代码
```
.grid {
display: -webkit-flex;
display: -moz-flex;
display: -o-flex;
display: -ms-flex;
display: flex;
}
.col {
padding: 30px;
}
.fluid {
flex: 1;
background:blue;
}
.fixed {
width: 400px;
background:red;
}
```
over.




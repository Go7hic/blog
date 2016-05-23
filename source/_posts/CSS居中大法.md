---
layout: post
title: CSS居中大法
date: 2014-02-24 11:21:04
tags: css
---

各种方法各有利弊，大家自己权衡吧，至少在需要居中时多个思路。

一：<center>

<center></center>，不建议用了。 
```
<center>水平居中</center>
```
二：text-align:center

在父容器里水平居中 inline 文字，或 inline 元素 

```
h2 {text-align:center;}
```

三：vertical-align:middle

vertical-align适用于 inline level, inline-block level 及 table-cells 元素上。

```
<div class="middle">
	<strong>对象的内容与对象中部对齐</strong>
	<p>参考内容<a href="?">要对齐的内容</a></p>
</div>

p{border:1px solid #000;font-size:16px;line-height:2;}
a{margin-left:5px;font-size:12px;}
.middle a {
    vertical-align:middle;
}
```

四：line-height

与 height 联手，垂直居中文字.
```
<div class="middle">
	<p>参考内容,水平垂直居中</p>
</div>

.middle {
    width: 500px;
    height: 50px;
    padding: 20px 0;
    border: 1px solid red;
}
p {
    height: 10px;
    line-height: 10px;
    text-align: center;
}
```
####五：margin
```
<div class="box">
  <div class="box2">Hello World</div>
</div>
.box { 
    width:200px; 
    background-color:yellow;
}
.box2{
     margin:0px auto; 
    background-color:gray; 
    color:white; 
    display:table; 
}
```

####六：translate(-50%,-50%)

这个就很厉害了，用 position 加 translate translate(-50%,-50%) 比较奇特，百分比计算不是以父元素为基准，而是以自己为基准。同样适用于没固定大小的内容，min-width，max-height，overflow:scroll等。 示例：
```
<div class="container">
	<div class="content">hello world</div>
</div>
.container { 
    width:200px; 
    height:200px;
    background-color:yellow; 
    position:relative; 
}
  .content { 
    position:absolute;
    left:50%; 
    top:50%; 
    transform:translate(-50%,-50%); 
    -webkit-transform:translate(-50%,-50%); 
    background-color:gray; 
    color:white; 
}
```
七:绝对定位居中
注意：高度必须定义，建议加 overflow: auto，防止内容溢出。
```
<div class="container">
</div>

.container {
	position: absolute;
	top: 0;
	left: 0;
	bottom: 0;
	right: 0;
	width: 50%;
	height: 50%;
	overflow: auto;
	margin: auto;
	background: blue;

}
```
八：负 margin

确切知道宽高，负 margin 是宽和高的一半。 
```
<div class="container">
	
</div>

.container {
	position: absolute;
	top: 50%; left: 50%;
	width: 300px;
	height: 200px;
	padding: 20px;
	margin-left: -170px; /* (width + padding)/2 */
	margin-top: -120px; /* (height + padding)/2 */
	background: blue;
}
```
####九：Table-Cell
 该方案是有局限性的，因为IE8以下的浏览器不支持 display 的table系value，所以你只能在IE8及以上浏览器以及非IE浏览器下才能看到效果；
参考文章：Flexible height vertical centering with CSS, beyond IE7

```
<div class="container">
	<p>你们好</p>
</div>
.container {
	display:table;
	width:500px;
	margin:10px auto;
	background:#eee;
}
.container p {
	display:table-cell;
	height:100px;
	vertical-align:middle;
	text-align: center;
}
```

####十:FlexBox

```
<div class="container">
<h1>你们好</h1>
</div>

.container {
	display: -webkit-box;
	display: -moz-box;
	display: -ms-flexbox;
	display: -webkit-flex;
	display: flex;
	-webkit-box-align: center;
	-moz-box-align: center;
	-ms-flex-align: center;
	-webkit-align-items: center;
	align-items: center;
	-webkit-box-pack: center;
	-moz-box-pack: center;
	-ms-flex-pack: center;
	-webkit-justify-content: center;
	justify-content: center;
	height: 100px;
	background: #222;
	color: red;
}
```

十一： display:inine-block

```
<div class="container">
	<p>这是display:inline-block实现水平垂直居中的终解</p>
</div>
.container {
	width: 200px;
	height:200px;
	text-align:center;
	font-size:0;
	background: blue;
}
.container:after {
	content: ".";
	display:inline-block;
	*display:inline;
	*zoom:1;
	width:0;
	height:100%;
	vertical-align:middle;
}

p {
	display:inline-block;
	*display:inline;
	*zoom:1;
	vertical-align:middle;
	font-size:16px;
}
```
十二 ： writing-mode大法

```
<div class="container">
	<p>writing-mode法</p>
</div>

.container{
	width: 500px;

	height: 500px;
	line-height: 500px;
	text-align: center;
	background: #B9D6FF;
	border: 1px solid #f00;
}
p {
	height: 100%\9;
	writing-mode: tb-rl\9;
	vertical-align: middle;
}
```

####十三: 诡异button大法

```
<button><span>这方法是要逆天？这里还可以放图片</span></button>

button {
 width: 400px;
 height: 400px;     
 background: red;
 border: 1px solid #ccc;    
}
```

####十四: 高端大气的 SVG大法

```
<div>
<svg>
<text x="50%" y="50%" dy=".3em">
	Look, I’m centered!
</text>
</svg>
</div>

div {
	width: 300px;
	height: 150px;
	background: #f06;
	font: bold 150% sans-serif;
	text-shadow: 0 1px 2px rgba(0,0,0,.5);
	overflow: hidden; resize: both; /* just for this demo */
	color: white;
}

svg {
	width: 100%; height: 100%;
	pointer-events: none; /* so that you can resize the element */
}

text {
	text-anchor: middle;
	pointer-events: auto; /* Cancel the svg’s pointer-events */
	fill: currentColor;
}
```




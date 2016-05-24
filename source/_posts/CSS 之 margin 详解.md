---
layout: post
title: css之 margin 详解
date: 2013-12-07 11:21:04
tags: CSS
---

mmargin大家应该都知道，我们通常称之为外边距，是css盒模型的基本组成部分。平常写css经常要用到margin，所以难免会出些问题。我们主要是注意以下几个地方：
margin的auto值；margin的百分比值；margin和相对偏移top, right, bottom, left的异同；margin叠加；

————0x01———————
首先我们看下auto，auot 是margin的可选值之一。像这个margin:auto 和margin:0 auto，我们应该经常看得见.这个是让我们的块元素水平居中的。举个例子：
css
```
#demo{margin:0 auto;
width:500px;
}
```
HTML
```
<div id=”deom”><p>嗨，我是margin</p></div>
```

事实上这个效果和margin:auto的效果是一样的，都是让 #demo 水平居中了，但纵向并没有任何变化。
因为我们知道margin:auto其实就是margin:auto auto auto auto,而margin:0 auto 其实就是margin:0 auto 0 auto,分别对应的是上右下左.因为根据规范根据规范，margin-top: auto; 和 margin-bottom: auto;，其计算值为0。所以，这就是为什么上面两个效果是一样的了。那么为什么 margin: auto; 和 margin: 0 auto; 能实现水平居中了。
因为左右方向的auto值均分了可用空间，使得块元素得以在包含块内居中显示.那么margin:auto能不能纵向垂直居中呢，不能！因为纵向是可以无限延展的，所以没有一个一定的值可以被参照被用来计算。

———-0x02———–
我们再看一下margin的百分比用法，这个很简单，但是有个地方容易弄错，同样举个例子：
假如一个div块级容器，width为1000px,height为600px,div的子元素定义margin:10%,5%;那最后你说margin的top,right,bottom,left计算值最终是多少? 我一开始也觉得是100px 30px 100px 30px，;可事实错的，看代码：
css
```
#demo{
width: 1000px;
height: 600px;
}

#demo p{
margin: 10% 5%;
}
```
HTML

```
<div id=”demo”><p>嗨，我是margin</p></div>
```

你在浏览器看看效果就会发现，事实告诉我们结果是 100px 50px 100px 50px.那为什么会这样的呢？因为规范中注明 margin 的百分比值参照其包含块的宽度进行计算。当然还有一种情况，就是当书写模式变成纵向时，它会参照其包含块的高度进行计算，这个也适用于前面的auto，一般情况下我们得到书写模式都是横向的多吧…

—————0x03—————–
除了以上，我们还经常会用到margin-top,margin-left等四个来对一些元素进行偏移，其实很多时候我们还会用到相对定位来进行对一些元素偏移，这里也讲一下margin的偏移与相对偏移的异同.当然还是通过那个可爱的例子来说明啦。比如我们想让一个div向下偏移50个像素，可以这样：
css:
```
#demo .margin-top{
margin-top: 50px;
}
#demo .relative-top{
position:relative;
top: 50px;
}
```
HTML:
```
<div id=”demo”>
<div>我是margin-top:50px</div>
<div>我是relative top:50px</div>
</div>
```
在浏览器打开你会发现他们的效果是一样的,是的，也就是效果一样，其实它两真的不一样。CSS中的margin用来添加盒子之间的水平和垂直间隙；而一个元素的position属性值如果不为static则发生定位，定位元素会产生定位盒，并且会根据 top, right, bottom, left 这4个物理属性进行排版布局。
回到上面这个例子
我们看到2个方法都可以做到向下偏移50px，不过它们的意义不太一样。
margin的top: 让该div的顶部与其相邻的元素（这里即其包含块）留有50px的空白。
relative的top: 让该div距离其包含块顶部边缘50px，因为是 relative ，所以这里是距离div自己的顶部边缘。。。

————–0x04———————-
最后再看一下一个很常见也很重要的margin外边距叠加效果
这次我们先来段可爱的代码:
css:
```
p{
margin:50px;
}
```
html:
```
<div id=”demo”>
<p>虽然字少，但我也是一个段落，嘻嘻</p>
<p>特么我也是一个段落啊</p>
<p>难道我就不是一个段落了么</p>
</div>
```
按我们的感觉，觉得这3p啊，肯定会发生点什么。是会真的合体在一起piapia，还是会分开来呢？嘻嘻`自己用浏览器写写看下效果吧。嗯，你会发现，他们合体了，每个p之间都是50px,怎么不是我们想想象中的100px?这就是margin的叠加.
用简单的文字来说就是：当两个或更多的垂直外边距（margin-top/margin-botom）相遇时，他们将形成一个外边距，而这个外边距的高度就等于两个发生叠加的外边距的高度中那个值较大的，当然如果两个相等就随便等于谁啦，就像上面例子一样..

这里还得注意一点，就是外边距是可以与本身发生叠加的，假如有一个空元素，它有外边距，但是没边框或内边距，这时候顶外边距与底外边距就碰到一起来了，叠加就发生了..
这也就是很多时候看到一系列的段落元素占用的空间非常小的原因，因为他们的外边距都叠加起来了，只形成一个小的外边距.
最后我们想下，浮动元素会发生叠加么？试一下把上面那段css代码换位
```
p{
float: left;
margin: 50px;
}
```

你再看下效果，发现木有叠加哦…
是的,《精通css》一书里写道，只有普通文档流中块框的垂直外边距才会发生外边距叠加，行内框，浮动框或绝对定位框之间的外边距不会叠加.

再顺带提一下，和前面的类似，在水平书写模式下，发生 margin 折叠的是垂直方向，即 margin-top/margin-bottom，在垂直书写模式下，margin 折叠发生在水平方向上，即 margin-right/margin-left。

既然我们知道了他是怎么叠加的，那么避免margin叠加就容易了，我们可以把它改成非块元素，让它浮动，让它绝对定位，让它 overflow:hidden ，用边框隔开它们。

over…
更多css margin资料参考：http://www.ituring.com.cn/minibook/1024




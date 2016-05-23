---
layout: post
title: CSS实现宽度和高度等比列放大缩小效果的一个技巧
date: 2014-03-22 11:21:04
tags: css
---

先上 Demo：http://jsfiddle.net/gothic/tGv7G/light/

```
<div class="box">
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
</div>
```

```
.item {
    width:40%;
    height:0;
    padding-bottom: 80%;
    background-color:#f00;
    float:left;
    margin:10px 5px;;
}
```

这个效果要求是每个item元素的高度都是宽度的2倍，我们首先父元素box设置了宽度100%，这里我们主要利用padding的一个属性来解决问题，因为padding的宽度如果是百分数来计算的的话，那么它的实际值都是相对父元素的宽度来算百分数的值，包括 padding-bottom 和 padding-top 也是如此，所以我们这里宽度可以设置为40%。由于我们这里box的宽度是100%，而高度没有告诉，所以不能直接设置高度值来取得效果，我可利用padding-bottom来代替height值，即如上所示，把height设为0，而把padding-bottom设为80%，这样我们就可以看到效果了，你可以随意拖放浏览器窗口大小，都是等比列缩小放大的哦这个也算是自适应屏幕的一个小方法吧》。。




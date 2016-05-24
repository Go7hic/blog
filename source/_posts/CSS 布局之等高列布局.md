---
layout: post
title: css布局之等高列布局
date: 2014-02-16 11:21:04
tags: CSS
---
前面分别回顾了两栏三栏布局，今天继续回顾等高列布局，这个布局也很麻烦有两列三列多列，我就随便回顾一点了。

####一：用背景图片做假等高列

这种方法是我们实现等高列最早使用的一种方法，就是使用背景图片，在列的父元素上使用这个背景图进行Y轴的铺放，从而实现一种等高列的假像。
优点：
实现方法简单，兼容性强，不需要太多的css样式就可以轻松实现。

缺点：
使用这种方法不适合流体布局等高列的布局，另外如果你需要更换背景色或实现其他列数的等高列时，都需要重新制作过背景图。

```
<div id="container">
  <div id="left">left</div>
  <div id="main">main</div>
  <div id="right">right</div>
</div>
```
```
*{margin:0;padding:0;}
#container {
  background: url("http://sandbox.runjs.cn/uploads/rs/360/sodrxytj/articlefct.GIF") repeat-y;
  width: 500px;
  height: 600px;
  margin: 0 auto;
}
#left {
  float: left;
  width: 110px;
}
#main {
  float: left;
  width: 222px;
}
#right {
  float:left;
  width: 168px;
}
```

####二：给容器div使用单独的背景色（固定布局），用三个中最高的容器撑开背景。 
这个原理就是，将内容都放在最里面的div里，这样，3列内容中列高最高的那个就会吧最里面的div撑成那个高度，然后这个div又被嵌套近外面的div，整体是嵌套关系，所以里面的告诉会影响外面的高度，造成等高。这种方法，你有几列，就要嵌套几层，有点麻烦。 

优点： 这种方法是不需要借助其他东西（javascript,背景图等）,而是纯CSS和HTML实现的等高列布局，并且能兼容所有浏览器（包括IE6），并且可以很容易创建任意列数。
 
缺点： 这种方法不像其他方法一样简单明了，给你理解会带来一定难度，但是只要你理解清楚了，将能帮你创建任意列数的等高布局效果。

```
<div class="container">
  <div class="rightWrap">
    <div class="contentWrap">
      <div class="leftWrap">
        <div class="aside column leftSidebar" id="left"><br/></div>
        <div id="content" class="column section"><br/><br/><br/></div>
        <div class="aside rightSidebat column" id="right"><br/><br/></div>
      </div>
    </div>
  </div>
</div>
```
```
 *{margin: 0;padding:0;}
  .container {
    width: 960px;
    margin: 0 auto;
  }
  .rightWrap {
    width: 100%;
    float: left;
    background: green;
    overflow: hidden;
    position: relative;
  }
  .contentWrap {
    float: left;
    background: orange;
    width: 100%;
    position: relative;
    right: 320px;/*此值等于rightSidebar的宽度*/
  }
  .leftWrap{
    width: 100%;
    background: lime;
    float:left;
    position: relative;
    right: 420px;/*此值等于Content的宽度*/
  }
  #left {
    float: left;
    width: 220px;
    overflow: hidden;
    position: relative;
    left: 740px;
  }
  #content {
    float: left;
    width: 420px;
    overflow: hidden;
    position:relative;
    left: 740px;
  }
  #right {
    float: left;
    overflow: hidden;
    width: 320px;
    position: #333;
    position: relative;
    left: 740px;
  }
  ```
  
####三、给容器div使用单独的背景色（流体布局）

这种布局可以说是就是第二种布局方法，只是这里是一种多列的流体等高列的布局方法。前面也说过了，其实现原理就是给每一列添加相对应用的容器，并进行相互嵌套，并在每个容器中设置背景色。这里需要提醒大家你有多少列就需要多少个容器，比如说我们说的三列，那么你就需要使用三个容器。

优点：
兼容各浏览器，可以制作流体等高列，交无列数限制。

缺点：
标签使用较多，结构过于复杂，不易于理解，不过你掌握了其原理也就不难了，这也不算太大缺点。
```
<div id="container3">
    <div id="container2">
        <div id="container1">
            <div id="col1">Column 1</div>
            <div id="col2">Column 2</div>
            <div id="col3">Column 3</div>
        </div>
    </div>
</div>
```

```
#container3 {
    float: left;
    width: 100%;
    background: green;/**/
    overflow: hidden;
    position: relative;
}
                                     
#container2 {
    float: left;
    width: 100%;
    background: yellow;
    position: relative;
    right: 30%; /*大小等于col3的宽度*/
}
 
#container1 {
    float: left;
    width: 100%;
    background: orange;
    position: relative;
    right: 40%;/*大小等于col2的宽度*/
}
 
#col1 {
    float:left;
    width:26%;/*增加了2%的padding，所以宽度减少4%*/
    position: relative;
    left: 72%;/*距左边呀增加2%就成72%*/
    overflow: hidden;
}
#col2 {
    float:left;
    width:36%;/*增加了2%的padding，所以宽度减少4%*/
    position: relative;
    left: 76%;/*距左边有三个padding为2%,所以距离变成76%*/
    overflow: hidden;
}
#col3 {
    float:left;
    width:26%;/*增加了2%的padding，所以宽度减少4%*/
    position: relative;
    left: 80%;/*距左边5个padding为2%，所以距离变成80%*/
    overflow: hidden;
}
```

####四、使用正padding和负margin对冲实现多列布局方法

就是先把列的padding-bottom设置一个足够大的值，然后在加一个margin-bottom，值为与padding-bottom的值正负相抵消的值。父容器设置overflow:hidden;。
这样子父容器的高度就还是它里面的列没有设定padding-bottom时的高度，当它里面的任一列高度增加了，则父容器的高度被撑到它里面最高那列的高度，其他比这列矮的列则会用它们的padding-bottom来补偿这部分高度差。
因为背景是可以用在padding占用的空间里的，而且边框也是跟随padding变化的，就实现了等高的效果。 
优点： 这种可能实现多列等高布局，并且也能实现列与列之间分隔线效果，结构简单，兼容所有浏览器 
缺点： 这种方法存在一个很大的缺陷，那就是如果要实现每列四周有边框效果，那么每列的底部（或顶部）将无法有边框效果。 下面我们就针对这个缺陷来介绍两种解决办法，第一种是使用背景图来模仿底部（或顶部）边框；第二种方法是使用div来模仿列的边框，图片有局限，我们只介绍用div的。

```
<div id="wrapper">
  <div id="left">
    <br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
    <div id="left-border"></div>
  </div>
  <div id="main">
    <br/><br/><br/><br/><br/>
    <div id="main-border"></div>
  </div>
 
  <div id="right">
    <br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
    <br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
    <div id="right-border"></div>
  </div>
</div>
```

```
 *{margin:0;padding:0;}
  #wrapper{
    margin: 0 auto;
    width: 960px;
    overflow: hidden;
    position: relative;
    _zoom: 1;
  }
  #left{
    border: 1px solid blue;
    float: left;
    width: 238px;
    background-color: yellow;
    padding-bottom: 30000px;
    margin-bottom: -30000px;
  }
  #main{
    border: 1px solid blue;
    float: left;
    width: 498px;
    padding-bottom: 30000px;
    margin-bottom: -30000px;
    background-color: green;
  }
  #right{
    border: 1px solid blue;
    float: left;
    width: 218px;
    padding-bottom: 30000px;
    margin-bottom: -30000px;
    background-color: red;
  }
  #left-border{
    position: absolute;
    bottom: 0;
    left: 0;
    height: 1px;
    background-color: blue;
    width: 240px;
  }
  #main-border{
    position: absolute;
    bottom: 0;
    left: 240px;
    height: 1px;
    background-color: blue;
    width: 500px;
  }
  #right-border{
    position: absolute;
    bottom: 0;
    right: 0;
    height: 1px;
    background-color: blue;
    width: 220px;
  }
  ```
  
####五、使用边框和定位模拟列等高

这种方法是使用边框和绝对定位来实现一个假的高度相等列的效果。假设你需要实现一个两列等高布局，侧栏高度要和主内容高度相等。

优点：
结构简单，兼容各浏览器，容易掌握。

缺点：
这个方法就是无法单独给主内容列设置背景色，并且实现多列效果效果不佳

```
<div id="wrapper">
<div id="mainContent">...</div>
<div id="sidebar">...</div>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
</div>
```

```
    #wrapper {
        width: 960px;
        margin: 0 auto;
    }
    #mainContent {
        border-right: 220px solid #dfdfdf;
        position: absolute;
        width: 740px;
    background:blue;
    }
    #sidebar {
        background: #dfdfdf;
        margin-left: 740px;
        position: absolute;
        width: 220px;
    background:red;
    }
```




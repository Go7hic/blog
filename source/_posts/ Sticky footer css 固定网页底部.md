---
layout: post
title: Sticky footer css 固定网页底部
date: 2015-01-03 11:21:04
tags: CSS
---

>经常看到一些网站页面内容比较少的时候，底部也往上走了，使得网站看上去很不和谐，起码我个人不喜欢这样看的。
今天总结了几个常用的方法来固定这个底部内容在页面，当然不是 position: fixed 那个固定了。

###第一个是我现在比较喜欢用的，代码结构简单，使用 css 伪元素

Html

```
<div class="page-wrap">
    content
</div>
<footer class="site-footer">
    我要固定在底部
</footer>
```

css

```
 * {
        margin: 0;
    }
    html,body {
        height: 100%;
    }
    .page-wrap {
        min-height: 100%;
        margin-bottom: -142px;
    }
    .page-wrap:after {
        content: "";
        display: block;
    }
    .site-footer, .page-wrap:after {
        height: 142px;
    }
    .site-footer {
        background-color: #f00;
    }
```
demo
<iframe width="100%" height="300" src="http://jsfiddle.net/gothic/x2gxxxwh/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

####第二个方法是之前用的，代码结构多套了一层，不过兼容性比较好
html
```
<div id="wrap">
    <div id="header">
    </div>
    <div id="main">
    </div>
</div>
<div id="footer">
</div>
```
css
```
html, body {height: 100%;}
#wrap {min-height: 100%;}
#main {overflow:auto;padding-bottom: 150px;} /* 要与footer的高度相同 */
#footer {position: relative;margin-top: -150px;height: 150px;clear:both;}
/*Opera 兼容*/
body:before {content:"";height:100%;float:left;width:0;margin-top:-32767px;}
```
没有 demo
###第三个方法和上面这个差不多，就是代码结构稍微变了下
html
```
<div id="container">
<div id="content"></div>
</div>
<div id="footer">固定于底部的内容</div>
```
css
```
html,body, #container { height: 100%; }
body > #container { height: auto; min-height: 100%; } // ie6不支持min-height，height:auto是必须的
#footer {clear: both;position: relative;z-index: 10;height:30px;margin-top: -30px;}//注意height的值与margin-top的数据值
#content { padding-bottom:30px; }  //注意这里，一定要padding-bottom的值为footer的高度
```
没有 demo

### 第四个方法是绝对定位方法
Demo
<iframe width="100%" height="300" src="http://jsfiddle.net/gothic/sLyfL5mz/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Html
```
<nav></nav>
<article>Lorem ipsum...</article>
<footer>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
    quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
    consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
    cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
    proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
</footer>
```

css

```
  * {
        margin: 0;
    }
html {
    position: relative;
    min-height: 100%;
}
body {
    margin: 0 0 100px; /* bottom = footer height */
}
footer {
    position: absolute;
    left: 0;
    bottom: 0;
    height: 100px;
    width: 100%;
}
```

###第5个方法暂时还不明白，先去折腾下 flex 再来，不过我试了代码没用
链接放这 http://philipwalton.github.io/solved-by-flexbox/demos/sticky-footer/




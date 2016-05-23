---
layout: post
title: Js 页面震动效果的几种实现方法
date: 2015-01-08 11:21:04
tags: JavaScript
---
wordprss 后台登录页面的登录框有个人性化效果，就是当你的登录信息填错了，登录框有个简单的震动效果。这个效果可以在我们平时用到的，觉得挺人性化的。具体怎么实现呢，我看了一下网上的方法，有用 jQuery 的，也有 jQuery UI的，也有原生 Js 实现的震动效果。

###1. jQuery 实现

html
```
    <div id="shaking">Click Me</div>
```
css
```
#shaking{
 position:relative;
 top:20px;
 left:30px;
 width:120px;
 height:120px;
 background-color:blue;
 color:white;
 font-weight:bold;
 text-align:center;
    line-height:120px;
}
```
jQuery
```
 $('#shaking').click(function(){
    if(!$(this).is(":animated")){
        $(this).animate({left:-8}, 210).animate({left:20}, 180)
        .animate({left:-4}, 150).animate({left:20}, 130)//是Chaining
        .animate({left:-2}, 100).animate({left:20}, 80);
    }
});
```
其实就是通过不断的元素左右的位置形成动画效果来实现这个震动的效果

这里还推荐一个不错的 jQuery 震动插件 [jrumble](https://github.com/jackrugile/jrumble)

####2.jQuery UI

html
```
<div id="demo">
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
    quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
    consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
    cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
    proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
</div>
```

js
```
function shake() {
        //100为震动间隔,3为震动次数
        $('#demo').effect('shake', { times: 3 }, 100);
    }
    jQuery(document).ready(function() {
        setInterval("shake()", 3000);
    });
```

####3.原生 js

html
```
<div id="box">
    <p>点我试试 Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
    quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
    consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
    cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
    proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
</div>
```
js
```
document.getElementById('box').onclick = function Shake() {
    var i = 20,
        _this = this;
    var Timer = setTimeout(active, 15)
    function active() {
        if (i >= 0) {
            _this.style.padding = 0;
            i%2 == 0?_this.style.paddingLeft = i + 'px': _this.style.paddingRight = i + 'px';
            i--
            Timer = setTimeout(active, 15)
        } else {
        clearTimout(Timer)
    }
    }
}
```




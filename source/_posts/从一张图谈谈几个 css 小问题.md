---
layout: post
title: 从一张图谈谈几个 css 小问题
date: 2015-05-26 11:21:04
tags: JavaScript
---

很久没写文章了，今天写一篇来充数。
在公司前端群里看到有人发了一张某同事写的 css 代码截图，看完后真的是醉了，我暂且不敢说那样写不好，但是我肯定是写不出那样的 css 的。
<a href="http://dyy.im/wp-content/uploads/2015/05/less.png"><img src="http://dyy.im/wp-content/uploads/2015/05/less-291x300.png" alt="less" width="291" height="300" class="alignnone size-medium wp-image-4613" /></a>
醉就醉了，过了一会看了一下他写的代码，发现里面一个问题。就是 当我们对某元素使用绝对定位 position: absolute;时，它已经脱了常规文章流，这时候对他设置 display 属性是无效的，应为它总是默认显示为 display: block ，所以截图代码里面那个绝对定位后面的  display: inline-block 是无效的。

说到 display 最近发现 display: none 和 visibility: hidden 有个不一样的地方我们很少注意到。平时我们说 display 和 visibility 有哪些不同点时一般是说 前者不占空间，后者占据空间，前者会回流和渲染后者不会，display:none隐藏产生reflow和repaint(回流与重绘)，而visibility:hidden没有这个影响前端性能的问题。一般就是这两个，但是我发现还有一个不同的地方， 就是 display: none 没有有继承性，而 visibility: hidden 没有，怎么说呢，具体效果就是如果父级元素设置了 display: none,子元素在设置 display:none 或 display: block，子元素都不会显示；但是如果父元素设置 visibility: hidden,子元素再设置 visibility: visible，子元素是可以看见的，并不会隐藏。具体可以看下面的效果：
<iframe width="100%" height="300" src="//jsfiddle.net/gothic/jeowynwd/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

再回到上面那个图，我们发现他的写法应该是为了 html精简好看加载快，减少 class。我在平时写样式的时候也有这样的纠结，就是如果 HTML里面的 class 等属性太多了，会导致 html 变大，加载的时候肯定会影响到一点点性能，而且一些不规范的 class 命名也会影响到点点性能，但是如果样式不通过选择器来设置样式，通过大量的元素标签来嵌套那显然是不可取的，一来维护困难，只要你的 html 结构一变，你的样式基本就没用了，二来这样的渲染效果也慢。所以我们写 html 和 css 时还是要两者结合，既不要让 HTML 看起来太臃肿，也不要让样式难以维护，这里面又涉及到 class 命名,css 写法的一些问题了，在此不写了。总之我觉得要写出好的结构样式也是要点能力的...




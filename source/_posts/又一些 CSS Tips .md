---
layout: post
title:  又一些 CSS Tips
date: 2015-10-23 11:21:04
tags: CSS
---
# CSS Tips

原文地址： https://github.com/AllThingsSmitty/css-protips



提示的收集，以帮助提升你的CSS技巧。

## 目录

* [专业的技巧](#protips)
* [支持情况](#support)

## 专业的技巧

1. [使用 `:not()` 选择器来决定导航是否显示边框](#use-not-to-applyunapply-borders-on-navigation)
1. [为 body 元素添加行高](#add-line-height-to-body)
1. [垂直居中任何元素](#vertically-center-anything)
1. [逗号分隔的列表](#comma-separated-lists)
1. [使用负的 `nth-child` 来选择元素](#select-items-using-negative-nth-child)
1. [使用 SVG 图标](#use-svg-for-icons)
1. [使用 “形似猫头鹰” 的选择器](#use-the-lobotomized-owl-selector)
1. [使用 `max-height` 来建立纯 CSS 的滑块](#use-max-height-for-pure-css-sliders)
1. [继承 `box-sizing`](#inherit-box-sizing)
1. [创造格子等宽的表格](#equal-width-table-cells)
1. [利用 Flexbox 去除多余的外边距](#get-rid-of-margin-hacks-with-flexbox)
1. [利用属性选择器来选择空链接](#use-attribute-selectors-with-empty-links)
1. [给 “预设” 链接定义样式](#style-default-links)
1. [一致的垂直节奏](#consistent-vertical-rhythm)
1. [内在比例盒](#intrinsic-ratio-boxes)
1. [为破碎图象定义样式](#style-broken-images)
1. [用 rem 来调整全局大小；用 em 来调整局部大小](#use-rem-for-global-sizing-use-em-for-local-sizing)
1. [隐藏没有静音、自动播放的影片](#hide-autoplay-videos-that-arent-muted)
1. [使用选择器 `:root` 来控制字体弹性](#use-root-for-flexible-type)
1. [将`font-size`关于表单元素为更好的移动体验](#set-font-size-on-form-elements-for-a-better-mobile-experience)


### 使用 `:not()` 选择器来决定导航是否显示边框

与其加上边框……

```css
/* 添加边框 */
.nav li {
  border-right: 1px solid #666;
}
```

……然后去掉最后一个元素的边框……

```css
/* 去掉边框 */
.nav li:last-child {
  border-right: none;
}
```

……不如使用 `:not()` 伪类来达成同样的效果：

```css
.nav li:not(:last-child) {
  border-right: 1px solid #666;
}
```

当然，也可以使用 `.nav li + li` 或者 `.nav li:first-child ~ li` 来达成，但是 `:not()` 选择器的方式更清晰明了，一目了然。


### 为 `body` 元素添加行高

不必为每一个 `<p>`，`<h*>`，等元素逐一添加 `line-height`，为 `body` 元素添加就好了：

```css
body {
  line-height: 1.5;
}
```

文本元素可以很自然地继承 `body` 的样式。


### 垂直居中任何元素

这不是黑魔法，真的可以垂直居中任何元素：

```css
html, body {
  height: 100%;
  margin: 0;
}

body {
  -webkit-align-items: center;  
  -ms-flex-align: center;  
  align-items: center;
  display: -webkit-flex;
  display: flex;
}
```

这还不够？垂直方向，水平方向……任何元素，任何时间，任何地方？CSS-Tricks [有篇好文](https://css-tricks.com/centering-css-complete-guide/) 讲到了各种居中的技巧。

**注意：** IE11 对 flexbox 的支持[有点 bug](https://github.com/philipwalton/flexbugs#3-min-height-on-a-flex-container-wont-apply-to-its-flex-items)。


### 逗号分隔的列表

列表的每项都由逗号分隔：

```css
ul > li:not(:last-child)::after {
  content: ",";
}
```

使用了 `:not()` 伪类，因此最后一项没加逗号。

**注意：**这一技巧对于无障碍，屏幕阅读器而言并不理想，需要注意。


### 使用负的 `nth-child` 来选择元素

使用负的 `nth-child` 可以选择 1 至 n 个元素。


```css
li {
  display: none;
}

/* 选择第 1 至第 3 个元素并显示出来 */
li:nth-child(-n+3) {
  display: block;
}
```

或许你已经掌握了[如何使用 `:not()`](#use-not-to-applyunapply-borders-on-navigation)，试下这个：

```css
/* 选择第 1 至第 3 个元素并显示出来 */
li:not(:nth-child(-n+3)) {
  display: none;
}
```

就是这么简单。


### 使用 SVG 图标

没有理由不使用 SVG 图标：

```css
.logo {
  background: url("logo.svg");
}
```

SVG 在所有分辨率下都可以良好缩放，IE9+ 及其它所有浏览器都支持，丢掉你的 .png, .jpg, 或 .gif-jif-whatev 文件吧。

**注意：** 针对仅有图标的按钮，如果 SVG 没有加载成功的话，以下样式对无障碍有所帮助：

```css
.no-svg .icon-only:after {
  content: attr(aria-label);
}
```


### 使用 “形似猫头鹰” 的选择器

这个名字可能比较陌生，不过全局选择器 (`*`) 和 相邻兄弟选择器 (`+`) 一起使用，效果非凡：

```css
* + * {
  margin-top: 1.5em;
}
```

在此示例中，遵循其他元素的文档流中的所有元素将都接收上边距 `margin-top: 1.5em` 的样式。

更多 “形似猫头鹰”  的选择器，可参考 *A List Apart* 上面 [Heydon Pickering 的文章](http://alistapart.com/article/axiomatic-css-and-lobotomized-owls)


### 使用 `max-height` 来建立纯 CSS 的滑块

`max-height` 与 overflow hidden 一起来建立纯 CSS 的滑块：

```css
.slider {
  max-height: 200px;
  overflow-y: hidden;
  width: 300px;
}

.slider:hover {
  max-height: 600px;
  overflow-y: scroll;
}
```

移入滑块元素时增大它的 `max-height` 的值，便可以显示溢出部分。


### 继承 `box-sizing`

从 `html` 元素继承 `box-sizing` 就好：

```css
html {
  box-sizing: border-box;
}

*, *:before, *:after {
  box-sizing: inherit;
}
```

如此在插件或其它组件里改变 `box-sizing` 变得简单。


### 创造格子等宽的表格

`table-layout: fixed` 可以让每个格子保持等宽：

```css
.calendar {
  table-layout: fixed;
}
```

无痛的 table 布局。

### 利用 Flexbox 去除多余的外边距

与其使用 `nth-`， `first-`， 和 `last-child` 去除列之间多余的间隙，不如使用 flexbox 的 `space-between` 属性：

```css
.list {
  display: flex;
  justify-content: space-between;
}

.list .person {
  flex-basis: 23%;
}
```

列之间的间隙相等，并且首尾没有多余的间隙。


### 利用属性选择器来选择空链接

当 `<a>` 元素没有文本内容，但是有 `href` 属性的时候，可以显示它的 `href` 链接：

```css
a[href^="http"]:empty::before {
  content: attr(href);
}
```

相当简便。


### 给 “预设” 链接定义样式

给 “预设” 链接定义样式：

```css
a[href]:not([class]) {
  color: #008000;
  text-decoration: underline;
}
```

通过 CMS 系统插入的链接，通常没有 `class` 属性，以上样式可以甄别它们，而且不会影响其它样式。


### 一致的垂直节奏

通用选择器 (`*`) 跟元素一起使用，可以保持一致的垂直节奏：

```css
.intro > * {
  margin-bottom: 1.25rem;
}
```

一致的垂直节奏可以提供视觉美感，增强内容的可读性。


### 内在比例盒

要创建具有内在比一个盒子，所有你需要做的就是应用顶部或底部填充，从一个div：

```css
.container {
  height: 0;
  padding-bottom: 20%;
  position: relative;
}

.container div {
  border: 2px dashed #ddd;	
  height: 100%;
  left: 0;
  position: absolute;
  top: 0;
  width: 100%;
}
```

使用20％的填充使得框等于其宽度的20％的高度。不管视口的宽度，子元素的div将保持其宽高比（100％/ 20％= 5:1）。


### 为破碎图象定义样式

只要一点CSS就可以美化破碎的图象：

```css
img {  
  display: block;
  font-family: Helvetica, Arial, sans-serif;
  font-weight: 300;
  height: auto;
  line-height: 2;
  position: relative;
  text-align: center;
  width: 100%;
}
```

以添加伪元素的法则来显示使用者信息和虚线图像URL的引用：

```css
img:before {  
  content: "We're sorry, the image below is broken :(";
  display: block;
  margin-bottom: 10px;
}

img:after {  
  content: "(url: " attr(src) ")";
  display: block;
  font-size: 12px;
}
```

了解更多关于这类样式的技巧 [Ire Aderinokun](https://github.com/ireade/)的 [原帖](http://bitsofco.de/styling-broken-images/).

### 用 `rem` 来调整全局大小；用 `em` 来调整局部大小

在根设置基本字体大小后 (`html { font-size: 16px; }`), 请设置为文本元素的字体大小 `em`:

```css
h2 { 
  font-size: 2em;
}

p {
  font-size: 1em;
}
```

然后设置字体大小的模块 `rem`:

```css
article {
  font-size: 1.25rem;
}

aside .module {
  font-size: .9rem;
}
```

现在，每个模块变得条块分割，更容易、灵活的样式，以便于维护。

### 隐藏没有静音、自动播放的影片

这是一个自定义的使用者样式表一个伟大的把戏。避免在加载页面时自动播放。如果没有静音，则不显示视频：

```css
video[autoplay]:not([muted]) {
  display: none;
}
```

再次，我們趁著使用的 [`:not()`](#use-not-to-applyunapply-borders-on-navigation) 偽類。


### 使用选择器`:root`来控制字体弹性

在响应式布局的类型字体大小应能与每个视区进行调整。你可以计算的基础上视区高度的字体大小和宽度，使用`:root`:

```css
:root {
  font-size: calc(1vw + 1vh + .5vmin);
}
```

现在，您可以利用基于计算的值`root em`单位：`root`:

```css
body {
  font: 1em/1.6 sans-serif;
}
```


### 将`font-size`关于表单元素为更好的移动体验

为了避免在HTML表单元素缩放移动浏览器（Safari浏览器的iOS，_et AL_。）当一个`<选择>`下拉被窃听，加上`字体size`的选择规则：

```css
input[type="text"],
input[type="number"],
select,
textarea {
  font-size: 16px;
}
```


### 支持情况

这些技巧适用于最新版的 Chrome, Firefox, Safari, Opera, Edge, 以及 IE11。




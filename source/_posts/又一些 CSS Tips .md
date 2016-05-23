---
layout: post
title:  又一些 CSS Tips
date: 2015-10-23 11:21:04
tags: CSS
---
# CSS Tips
原文地址： https://github.com/AllThingsSmitty/css-protips


1. [Use `:not()` to Apply/Unapply Borders on Navigation](#use-not-to-applyunapply-borders-on-navigation)
1. [Add Line-Height to `body`](#add-line-height-to-body)
1. [Vertically-Center Anything](#vertically-center-anything)
1. [Comma-Separated Lists](#comma-separated-lists)
1. [Select Items Using Negative `nth-child`](#select-items-using-negative-nth-child)
1. [Use SVG for Icons](#use-svg-for-icons)
1. [Text Display Optimization](#text-display-optimization)
1. [Use `max-height` for Pure CSS Sliders](#use-max-height-for-pure-css-sliders)
1. [Inherit `box-sizing`](#inherit-box-sizing)
1. [Equal Width Table Cells](#equal-width-table-cells)
1. [Get Rid of Margin Hacks With Flexbox](#get-rid-of-margin-hacks-with-flexbox)
1. [Use Attribute Selectors with Empty Links](#use-attribute-selectors-with-empty-links)


### 用`:not()`给导航条之间添加间隔线

给元素添加边框

```css
/* add border */
.nav li {
  border-right: 1px solid #666;
}
```

...然后把最后一个元素的边框去掉...

```css
/* remove border */
.nav li:last-child {
  border-right: none;
}
```

...现在只需这样，给你想要应用的元素使用用 `:not()` 伪类选择器:

```css
.nav li:not(:last-child) {
  border-right: 1px solid #666;
}
```

当然，你也可以用 `.nav li + li` 或者 `.nav li:first-child ~ li` 实现, 但是用 `:not()` 非常简单。


### 给 body 元素增加 Line-Height 属性

我们不需要给每个 p、h1 元素设置 `line-height`，只需要给 body 元素设置，其他文本元素会自动继承body 的特性。

```css
body {
  line-height: 1;
}
```


### 垂直居中任何元素

不是黑魔法，确实可以让任意元素垂直居中。

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

Want to center something else? Vertically, horizontally...anything, anytime, anywhere? CSS-Tricks has [a nice write-up](https://css-tricks.com/centering-css-complete-guide/) on doing all of that.

**Note:** Watch for some [buggy behavior](https://github.com/philipwalton/flexbugs#3-min-height-on-a-flex-container-wont-apply-to-its-flex-items) with flexbox in IE11.


### 逗号分隔的列表

让列表项看起来像真的逗号分隔的列表

```css
ul > li:not(:last-child)::after {
  content: ",";
}
```

Use the `:not()` pseudo-class so no comma is added to the last item.


### 在 `nth-child` 中使用负数 

在css的nth-child中使用负数选择1~n条记录。
使用 `nth-child` 负数来选择 1到 n 的元素

```css
li {
  display: none;
}

/* select items 1 through 3 and display them */
li:nth-child(-n+3) {
  display: block;
}
```

或者如果你已经知道了一点点关于 [using `:not()`](#use-not-to-applyunapply-borders-on-navigation)，可以试试:

```css
/* select items 1 through 3 and display them */
li:not(:nth-child(-n+3)) {
  display: none;
}
```


### 图标使用 SVG

没有理由不用 SVG 图标:

```css
.logo {
  background: url("logo.svg");
}
```

SVG scales well for all resolution types and is supported in all browsers back to IE9. So ditch your .png, .jpg, or .gif-jif-whatev files.

**Note:** If you have SVG icon-only buttons for sighted users and the SVG fails to load, this will help maintain accessibility:

```css
.no-svg .icon-only:after {
  content: attr(aria-label);
}
```


### 文本显示优化

Sometimes fonts don't display optimally on all devices, so let the device browser help:

```css
html {
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility;
}
```

**Note:** [Please play with `optimizeLegibility` responsibly](https://bocoup.com/weblog/text-rendering/). Also, there's no `text-rendering` support for IE/Edge.


###  在 Pure CSS Sliders中使用 `max-height`

Implement CSS-only sliders using `max-height` with overflow hidden:

```css
.slider ul {
  max-height: 0;
  overlow: hidden;
}

.slider:hover ul {
  max-height: 1000px;
  transition: .3s ease; /* animate to max-height */
}
```


### 继承 `box-sizing`

Let `box-sizing` be inheritted from `html`:

```css
html {
  box-sizing: border-box;
}

*, *:before, *:after {
  box-sizing: inherit;
}

```

This makes it easier to change `box-sizing` in plugins or other components that leverage other behavior.


### 表格单元格等宽

Tables can be a pain to work with so try using `table-layout: fixed` to keep cells at equal width:

```css
.calendar {
  table-layout: fixed;
}
```

Pain-free table layouts.


### 使用Flexbox 消除各种 Margin Hacks

When working with column gutters you can get rid of `nth-`, `first-`, and `last-child` hacks by using flexbox's `space-between` property:

```css
.list {
  display: flex;
  justify-content: space-between;
}

.list .person {
  flex-basis: 23%;
}
```

Now column gutters always appear evenly-spaced.


### 给空连接使用属性选择符

Display links when the `<a>` element has no text value but the `href` attribute has a link: 

```css
a[href^="http"]:empty::before {
  content: attr(href);
}

```




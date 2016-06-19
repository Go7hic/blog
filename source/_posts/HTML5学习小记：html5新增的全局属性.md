
---
layout: post
title: HTML5 学习小记：HTML5 5新增的全局属性
date: 2013-06-04 11:21:04
tags: HTML5
---

刚刚谷歌了一下，发现我的和网上写的一些教程不一样，管它妹的，信自己。

·················割丁丁············

1.contenteditable属性：任何元素使用contenteditable属性的话，代表该元素是一个可编辑的区域。用户可以改变元素的内容以及操作标记。例如;

代码:
```
<!DOCTYPE html>
<head>
<meta charset=”utf-8″>
<title>conentEditable属性示例</title>
</head>
<h2>可编辑列表</h2>
<ul contentEditable=”true”>
<li>列表元素</li>
<li>列表元素2</li>
<li>列表元素3</li>
</ul>
```
具体效果自己新建上面的html代码用浏览器打开，即可

2.   designMode属性：表示是否整个页面上所有的支持contentEditable的元素变为可以编辑，这属性值只可以在js中被更改。用法如下：document.designmod=”on”

3.   hidden:如果被激活表示浏览器不渲染这个元素，但是其内容浏览器还是会创建这个元素中的内容

4.spellcheck:表示让textfield或者textarea中用户输入的文本内容进行拼写和语法检查，除非这个元素已经被设置为readOnly或者disabled

5.tabindex属性： tanindex是开发中的一个基本概念，就是当不断敲击tab键让窗口或页面中的控件获得焦点，对窗口或页面中的所有控件进行遍历的时候，每一个控件的tabindex表示该控件时被第几个访问的




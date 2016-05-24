---
layout: post
title: CSS Tips：美化上传按钮并获取上传文件路径
date: 2014-11-19 11:21:04
tags: CSS
---
上传功能很常见了，就是一个 input file 功能。

但是默认的上传很丑啊有没有，所以网上也有很多美化的 css 文章，但是很多也只是美化以下按钮而已，今天遇到一个问题就是还要显示上传文件的路径，一般默认的都会在后面显示上传文件名，但是按照网上的教程美化之后，就把文件名这些东西看不到了，这也是很郁闷的。所以这里等于有两个需求，一个是按钮要美化，另一个就是还要显示上传文件的路径。

首先我们来看一下默认的上传样式
 
 ```
 	<input type="file" >
 ```


选择上传文件后，后面会显示文件的名字，但是很多按钮美化的方法都把这个名字也隐藏掉了，所以这是不符合需求的。看一下美化后的

```
 <div class="report-file">
      <span>上传文件</span><input tabindex="3" size="3" name="report" class="file-prew" type="file" onchange="document.getElementById('textName').value=this.value">
 </div>
 <input type="text" id="textName" style="height: 28px;border:1px solid #f1f1f1" />
```
 
```
	.report-file {
            display: block;
            position: relative;
            width: 120px;
            height: 28px;
            overflow: hidden;
            border: 1px solid #428bca;
            background: none repeat scroll 0 0 #428bca;
            color: #fff;
            cursor: pointer;
            text-align: center;
            float: left;
            margin-right:5px;
	}
	.report-file span {
            cursor: pointer;
            display: block;
            line-height: 28px;
	}
	.file-prew {
            position: absolute;
            top: 0;
            left:0;
            width: 120px;
            height: 30px;
            font-size: 100px; 
            opacity: 0; 
            filter: alpha(opacity=0);
            cursor: pointer;
	}
```

 
 DEMO 
 
 可以看到每当我们上传一个文件后都会显示文件的路径，感觉这样会给用户一种更好的体验吧，相对那种直接一个上传按钮。
 
 <iframe width="100%" height="300" src="http://jsfiddle.net/gothic/ah62jejz/2/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>




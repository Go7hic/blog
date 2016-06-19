---
layout: post
title: 几个少数人知道的 CSS 知识
date: 2015-07-18 11:21:04
tags: CSS
---
### 1.border-radius 属性能用两组值
像这样：
.box {
	border-radius: 50px 50px /50px 50px
}

具体可以看 MDN 上面的介绍 https://developer.mozilla.org/en-US/docs/Web/CSS/Tools/Border-radius_generator

### 2.font-weight 属性接收相对值
怎么说，我是看到同事代码用这个才发现的，`font-weight: bolder`，当时我想这个属性的结果和 bold 有啥区别呢，其实从名字都能猜到，就是比粗更粗嘛，类是 `lighter` 也是比细更细。但是这个值是相对的，相对粗细本身的，如果你 css 里font-weight没用到 bold 和 light 或者 700,400,那么你用 bolder 和 lighter 效果和 bold 和 light 效果是一样的。 

可以看个简单的例子

```
.box {
  font-weight: 100;
}
 
.box-2 {
  font-weight: bolder; /* maps to 400 */
}
 
.box-3 {
  font-weight: bolder; /* maps to 700 */
}
 
.box-4 {
  font-weight: 400;
}
 
.box-5 {
  font-weight: bolder; /* maps to 700 */
}
 
.box-6 {
  font-weight: bolder; /* maps to 900 */
}
 
.box-7 {
  font-weight: 700;
}
 
.box-8 {
  font-weight: bolder; /* maps to 900 */
}
 
.box-9 {
  font-weight: bolder; /* maps to 900 */
}
 
.box-10 {
  font-weight: lighter; /* maps to 700 */
}
 
.box-11 {
  font-weight: lighter; /* maps to 400 */
}
 
.box-12 {
  font-weight: lighter; /* maps to 100 */
}
```

### 3. ouline-offset 属性
outline 新增了一个属性，outline-offset ，从名字应该可以猜到是 outline 相对 元素偏离的距离。
格式参考：

```
outline: 2px solid #ccc;
outline-offset: 15px;
```

### 4. 一些属性值是受大小写影响的

```
<div class="box"></div>
<input type="email">
```

有时候我们可能会用下面的方法来写样式

```
div[class="box"] {
  color: blue;
}
 
input[type="email"] {
  border: solid 1px red;
}
```
但是如果把 class 和 type 值换成大写会有什么效果呢，

```
div[class="BOX"] {
  color: blue;
}
 
input[type="EMAIL"] {
  border: solid 1px red;
}
```

会发现 class 值大小写的结果是一样的，但是 type 的结果不一样，也就是说 type 属性值是受大小写影响的。


### 5. 可以选择元素的范围
我们知道 css3 里有个 :nth-child 这个选择器，但是你可能不知道他可以用来选择一定范围内的元素。比如：

```
ol li:nth-child(n+7):nth-child(-n+14) {
  background: lightpink;
}
```

```
    <ol class="ol">
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
    <li>List item example</li>
  </ol>
 ```

  上面这个就是选中第七个到第十四个 li 元素。


### 6. css 动画效果可能因为动画名字而无效

 ```
 @keyframes reverse {
  from {
    left: 0;
  }
 
  to {
    left: 300px;
  }
}
.example {
  animation: reverse 2s 1s;
}
```

上面css 动画的名字叫做 reverse ，但是实际上这个动画效果是无效的。因为 reverse 是 css 动画效果的关键字， 类似保留名一样，类似的还有 infinite, alternate, running, paused，这几个是不能用来当做 css 动画名字的


### 7.强大的伪元素::first-letter

``` 
<div class="text">[还]好还好</div>
 <div class="text">"还"好还好</div>
 <div class="text">(还)好还好</div>
```

 ```
 .text::first-letter {color: #c00;}
 ```

上面的结果我们会发现第一个文字的样式会发生改变，而且添加一些符号对第一个文字也没影响。



最后可以看下在 jsfiddle 上的小 demo.

<iframe width="100%" height="300" src="//jsfiddle.net/jv80aq7h/1/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>



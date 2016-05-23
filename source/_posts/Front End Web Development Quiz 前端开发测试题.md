---
layout: post
title: Front End Web Development Quiz 前端开发测试题
date: 2014-11-04 11:21:04
tags: JavaScript
---
测试地址： http://davidshariff.com/quiz/

CSS篇

1.
```
ul {
MaRGin: 10px;
}
Are CSS property names case-sensitive?

Yes No Skip
```
选 NO。CSS的属性名对大小写并不敏感，但是我们一般都要求小写。

2.
```
Does setting margin-top and margin-bottom have an affect on an inline element?

Yes No Skip
```
选No。行内元素是不可以设置宽高长度尺寸的属性的

3.
```
Does setting padding-top and padding-bottom on an inline element add to its dimensions?

Yes No Skip
```
选 NO.同上

4.
```
If you have a

<p> element with font-size: 10rem, will the text be responsive when the user resizes / drags the browser window?

Yes No Skip
```
选 NO。rem 是相对网站根元素的一般是 HTML或body的属性值。

5.
```
The pseudo class :checked will select inputs with type radio or checkbox, but notelements.

True False Skip
```
选 False。会选中下拉列表的选项的。比如：
```
:checked {
  width: 50px;
  height: 50px;
}

input[type="radio"]:checked {
  margin-left: 25px;
}

input[type="checkbox"]:checked {
  display: none;  
}

option:checked {
  color: red;
}
```
6.
```
In a HTML document, the pseudo class :root always refers to the element.

True False Skip
```
选 True。这个没啥疑问的，根元素。。

7
```
.The translate() function can move the position of an element on the z-axis.

True False Skip
```
选False 。translate()是2D不能改变 Z轴的值。

8.
```
HTML:
<ul class="shopping-list" id="awesome">
    <li><span>Milk</span></li>
    <li class="favorite" id="must-buy"><span class="highlight">Sausage</span></li>
</ul>
CSS:
ul {
    color: red;
}
li {
    color: blue;
}
What is the color of the text Sausage ?

Red
Blue
Neither
Skip
```
选 Blue 。考查 css选择器权重的。

9.
```
HTML:

<ul class="shopping-list" id="awesome">
    <li><span>Milk</span></li>
    <li class="favorite" id="must-buy"><span class="highlight">Sausage</span></li>
</ul>
CSS:

ul li {
    color: red;
}
#must-buy {
    color: blue;
}
What is the color of the text Sausage ?

Red
Blue
Neither
Skip
```
选Blue ,还是css选择器的问题。

10.

Given the HTML below:
```
<ul class="shopping-list" id="awesome">
    <li><span>Milk</span></li>
    <li class="favorite" id="must-buy"><span class="highlight">Sausage</span></li>
</ul>
What is the color of the text Sausage ?

.shopping-list .favorite {
    color: red;
}
#must-buy {
    color: blue;
}
Red
Blue
Neither
Skip
```
选 Blue ，尼马还是 css选择其权重的问题。

11. 好像也是选 Blue，点的太快忘记题目了

12.

Given the HTML below:
```
<ul class="shopping-list" id="awesome">
    <li><span>Milk</span></li>
    <li class="favorite" id="must-buy"><span class="highlight">Sausage</span></li>
</ul>
What is the color of the text Sausage ?

ul#awesome #must-buy {
    color: red;
}
.favorite span {
    color: blue!important;
}
Red
Blue
Neither
Skip
```
还是选 Blue， 选择器权重问题。

13.

Given the HTML below:
```
<ul class="shopping-list" id="awesome">
    <li><span>Milk</span></li>
    <li class="favorite" id="must-buy"><span class="highlight">Sausage</span></li>
</ul>
What is the color of the text Sausage ?

ul.shopping-list li .highlight {
    color: red;
}
ul.shopping-list li .highlight:nth-of-type(odd) {
    color: blue;
}
Red
Blue
Neither
Skip
```
选 Blue，:nth-of-type(odd) css3选择器,表示选择奇数行，所以这里第一行也是唯一的一行会起作用。

14.

Given the HTML below:
```
<ul class="shopping-list" id="awesome">
    <li><span>Milk</span></li>
    <li class="favorite" id="must-buy"><span class="highlight">Sausage</span></li>
</ul>
What is the color of the text Sausage ?

#awesome .favorite:not(#awesome) .highlight {
    color: red;
}
#awesome .highlight:nth-of-type(1):nth-last-of-type(1) {
    color: blue;
}
Red
Blue
Neither
Skip
```
选 Red, :not css3选择器，匹配的是所有非指定元素，第一个样式规则比第二个样式规则权重高

15.

HTML:
```
<p id="example">Hello</p>
CSS:

#example {
    margin-bottom: -5px;
}
What will happen to the position of #example?

It will move 5px downwards
All elements succeeding #example with move 5px upwards
Neither
Skip
```
选第二个，p元素后面的元素会向上移动5个像素

16.
```
HTML:

<p id="example">Hello</p>
CSS:

#example {
    margin-left: -5px;
}
What will happen to the position of #example?

It will move 5px left
All elements preceding #example with move 5px to the right
Neither
Skip
```
选第一个，向左移动5个像素。

17.
```
HTML:

<div id="test1">
    <span id="test2"></span>
</div>
CSS:

#i-am-useless {
    background-image: url('mypic.jpg');
}
Are unused style resources still downloaded by the browser?

Yes
No
Sometimes
Skip
```
选 No, html里没有用到这个标签，也就是说这个样式在当前页面中是无效的，所以背景图片是不会加载的。

18.
```
HTML:

<div id="test1">
    <span id="test2"></span>
</div>
CSS:

#test2 {
    background-image: url('mypic.jpg');
    display: none;
}
On page load, will mypic.jpg get downloaded by the browser?

Yes
No
Skip
```
选择 Yes。 虽然这里有display:none，但是浏览器还是会加载背景图片的，因为css是自上而下解析的，这里还没有足够的信息说明test2不显示

19
```
HTML:

<div id="test1">
    <span id="test2"></span>
</div>
CSS:

#test1 {
    display: none;
}
#test2 {
    background-image: url('mypic.jpg');
    visibility: hidden;
}
On page load, will mypic.jpg get downloaded by the browser

Yes
No
Skip
```
答: 选 No. 与上面的一样, 不过这里可以很确定的是 test2将不显示。

20.
```
CSS:
@media only screen and (max-width: 1024px) {
    margin: 0;
}
What is the use of the only selector ?
```
only 选择器的作用是什么 ?

选 Stops older browsers from parsing the remainder of the selector.
是阻止旧浏览器解析后续的选择器的.
在 CSS3 Media Queries 中有如下描述:
The keyword ‘only’ can also be used to hide style sheets from older user agents. User agents must process media queries starting with ‘only’ as if the ‘only’ keyword was not present.
所以, 关键字 only 就是为了不让旧浏览器正确解析的.

21.
```
HTML:
<div>
    <p>I am floated</p>
    <p>So am I</p>
</div>
CSS:

div {
    overflow: hidden;
}
p {
    float: left;
}
Does overflow: hidden create a new block formatting context ?
overflow: hidden 会创建一个新的块级格式化上下文吗 ?
```
选 Yes. CSS2.1 9.4.1 Block formatting contexts 一节中有如下描述:
Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with ‘overflow’ other than ‘visible’ (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.

因此, 只要 overflow 的值不是 visible 时, 都会创建一个新的块级格式化上下文.

22.
```
CSS:
@media only screen and (max-width: 1024px) {
    margin: 0;
}
Does the screen keyword apply to the device’s physical screen or the browser’s viewport ?

关键字 screen 指的是设备的物理屏幕, 还是指的浏览器的 viewport ?

Device’s physical screen
Browser’s viewport
Skip
```
选 Browser’s viewport ，可以参考这个问题
http://stackoverflow.com/questions/8549529/what-is-the-difference-between-screen-and-only-screen-in-media-queries

####js篇：

1.
```
"1" + 2 + "3" + 4
10
1234
37
```
答案：1234，加法优先级等同，从左往右，数字与字符串相加，数字转换成字符串进行运算，结果等同于：”12″+”3″+4 = “123”+4 = “1234”。

2.
```
4 + 3 + 2 + "1"
10
4321
91
```
答案：91，优先级同上，从左往右，等同于：7+2+”1″ = 9+”1″ = “91”。

3.
```
var foo = 1;
function bar() {
    foo = 10;
    return;
    function foo() {}
}
bar();
alert(foo);
1
10
Function
undefined
Error
```
答案：1，function的定义会提前到当前作用域之前，所以等同于：
```
var foo = 1;
function bar() {
    function foo() {}
    foo = 10;
    return;
}
bar();
alert(foo);
```
所以，在foo=10的时候，foo是有定义的，属于局部变量，影响不到外层的foo。

参见：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope?redirectlocale=en-US&redirectslug=JavaScript%2FReference%2FFunctions_and_function_scope

Unlike functions defined by function expressions or by the Function constructor, a function defined by a function declaration can be used before the function declaration itself.
4.
```
function bar() {
    return foo;
    foo = 10;
    function foo() {}
    var foo = 11;
}
alert(typeof bar());
number
function
undefined
Error
```
答案：function，与上题类似，等同于：
```
function bar() {
    function foo() {}
    return foo;
    foo = 10;
    var foo = 11;
}
alert(typeof bar());
```
在return之后声明和赋值的foo都无效，所以返回了function。

参见：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/return

A function immediately stops at the point where return is called.

JS中function声明和var声明都会被提前，最终得到结果为function，是因为名称解析顺序-Name Resolution Order(http://t.cn/8kcIRts导致的function声明优先级大于var声明，而不是由return语句退出导致最后的结果~
5.
```
var x = 3;

var foo = {
    x: 2,
    baz: {
        x: 1,
        bar: function() {
            return this.x;
        }
    }
}

var go = foo.baz.bar;

alert(go());
alert(foo.baz.bar());
1,2
1,3
2,1
2,3
3,1
3,2
```
答案：3,1
this指向执行时刻的作用域，go的作用域是全局，所以相当于window，取到的就是window.x，也就是var x=3;这里定义的x。而foo.baz.bar()里面，this指向foo.baz，所以取到的是这个上面的x，也就是1。

参见：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this?redirectlocale=en-US&redirectslug=JavaScript%2FReference%2FOperators%2Fthis

6.
```
var x = 4,
    obj = {
        x: 3,
        bar: function() {
            var x = 2;
            setTimeout(function() {
                var x = 1;
                alert(this.x);
            }, 1000);
        }
    };
obj.bar();
1
2
3
4
undefined
```
答案：4，不管有这个setTimeout还是把这个函数立即执行，它里面这个function都是孤立的，this只能是全局的window，即使不延时，改成立即执行结果同样是4。

7.
```
x = 1;
function bar() {
    this.x = 2;
    return x;
}
var foo = new bar();
alert(foo.x);
1
2
undefined
```
答案：2，这里主要问题是最外面x的定义，试试把x=1改成x={}，结果会不同的。这是为什么呢？在把函数当作构造器使用的时候，如果手动返回了一个值，要看这个值是否简单类型，如果是，等同于不写返回，如果不是简单类型，得到的就是手动返回的值。如果，不手动写返回值，就会默认从原型创建一个对象用于返回。

参见：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new

8.
```
function foo(a) {
    alert(arguments.length);
}
foo(1, 2, 3);
1
2
3
undefined
```
答案3，arguments取的是实参的个数，而foo.length取的是形参个数。

参见：
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/arguments/length?redirectlocale=en-US&redirectslug=JavaScript%2FReference%2FFunctions_and_function_scope%2Farguments%2Flength

arguments.length provides the number of arguments actually passed to a function. This can be more or less than the defined parameter count (See Function.length).

9.
```
var foo = function bar() {};
alert(typeof bar);
function
object
undefined
```
答案：undefined，这种情况下bar的名字从外部不可见，那是不是这个名字别人就没法知道了呢？不是，toString就可以看到它，比如说alert(foo)，可以看看能打出什么。

参见：
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope?redirectlocale=en-US&redirectslug=JavaScript%2FReference%2FFunctions_and_function_scope

The function name can be used only within the function’s body. Attempting to use it outside the function’s body results in an error (or undefined if the function name was previously declared via a var statement).

10.
```
var arr = [];
arr[0]  = 'a';
arr[1]  = 'b';
arr.foo = 'c';
alert(arr.length);
1
2
3
undefined
```
答案：2，数组的原型是Object，所以可以像其他类型一样附加属性，不影响其固有性质。

11.
```
function foo(a) {
    arguments[0] = 2;
    alert(a);
}
foo(1);
1
2
undefined
```
答案：2，实参可以直接从arguments数组中修改。

参见：
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/arguments?redirectlocale=en-US&redirectslug=JavaScript%2FReference%2FFunctions_and_function_scope%2Farguments

The arguments can also be set
12.
```
function foo(){}
delete foo.length;
alert(typeof foo.length);
number
undefined
object
Error
```
答案：number，foo.length是无法删除的，它在Function原型上，重点它的configurable是false。

参见：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete

delete can’t remove certain properties of predefined objects (like Object, Array, Math etc). These are described in ECMAScript 5 and later as non-configurable




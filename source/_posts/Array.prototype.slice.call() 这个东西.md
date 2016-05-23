---
layout: post
title: Array.prototype.slice.call() 这个东西
date: 2015-07-09 11:21:04
tags: JavaScript
---
今天在 mdn 上看文档，看到一个陌生的东西，仔细看了一下感觉还是不错的，这个东西就是 Array.prototype.slice.call()。
这个东西有啥用呢？一句话概括就是把类数组转成真的数组，怎么说呢，具体例子就是把 arguments 转成真正的数组。

我们知道 argument 对象是函数内部的本地变量，并且已经不是函数的属性了。他并不是一个真正的数组，他是一个类数组，没有数组特有的方法和属性，除了 length。我们可以通过 arguments 来获取函数所有的参数。这个对象为传递给函数的每个参数建立一个条目，条目的索引号从 0 开始。例如，如果一个函数有三个参数，你可以通过以下方式获取参数：
```
arguments[0]
arguments[1]
arguments[2]
```
很多时候我们需要把 arguments 里的内容转成数组来处理更方便，这时候我们就可以用这个方法来处理 arguments. 
`var args = Array.prototype.slice.call(arguments);`

我们还知道 slice 方法是数组自带的，所以通过 Array.prototype 来调用，在某些情况下也可以直接用 Array.slice 。我们再来分析一下， call() 方法在使用一个指定的 this 值和若干个指定的参数值的 v 前提下调用某个函数或方法。通过 call() 方法我们可以在一个对象上借用另一个对象上的方法，比如
Array.prototype.slice.call(arguments) 就是把 arguments 对象借用 Array 的 slice 方法，从而生成一个新的数组。我这里我就不继续分析 call() 方法了。

最后我们可以看一个例子：
```
function myConcat(separator) {
  var args = Array.prototype.slice.call(arguments, 1);
  return args.join(separator);
}
```
myConcat 只接收一个参数，现在我想传多个参数进去，myConcat(',','1','2','3')，我们前面说了，虽然 arguments 不是数组，但是有数组的 length 属性，所以上面函数里的1，就是从 arguments 参数里下表是1的参数开始，也就是第二个参数。所以我们可以得出 args = [1,2,3];因为函数只能接收一个参数，所以最后 return 里面的 separator为 `,`,最后通过 join 字符串拼接得到的就是 '1,2,3'了.
```
myConcat(',' ,'1','2','3') //'1,2,3'
```

可以参考 so 上面的一个回答:
 http://stackoverflow.com/questions/7056925/how-does-array-prototype-slice-call-work
 MDN 关于 call 的介绍
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call


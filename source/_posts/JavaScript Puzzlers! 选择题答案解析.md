---
layout: post
title: JavaScript Puzzlers! 选择题答案解析
date: 2014-08-12 11:21:04
tags: JavaScript
---

http://javascript-puzzlers.herokuapp.com/  题目地址。感觉很难，很多错了，边做边参考了一些资料。顺便参考网上答案自己整理一份答案在此。

第一题

What is the result of this expression? (or multiple ones)

```
["1", "2", "3"].map(parseInt) 
```
A：["1", "2", "3"]

B：[1, 2, 3]

C：[0, 1, 2]

D：other

解答：这里考的是map、parseInt的用法。map会传递三个参数给其作为参数的函数，为(element, index, array)，分别为当前的元素、当前元素在数组中的位置、整个数组：

> ["1", "2", "3"].map(function(){console.log(arguments)}) 
["1", 0, Array[3]]
["2", 1, Array[3]]
["3", 2, Array[3]]
而parseInt只接收两个参数，为(element, radix)，element代表需要被转换为int的字符串，radix代表当前字符串里数字的进制数

所以相当于说，结果数组的元素实际分别为为：
```
parseInt("1", 0)
parseInt("2", 1)
parseInt("3", 2)
```
parseInt("1", 0)的值为1，MDN上可以看到parseInt函数的radix为0时的行为
>
If radix is undefined or 0 (or absent), JavaScript assumes the following:
If the input string begins with "0x" or "0X", radix is 16 (hexadecimal) and the remainder of the string is parsed.
If the input string begins with "0", radix is eight (octal) or 10 (decimal). Exactly which radix is chosen is implementation-dependent. ECMAScript 5 specifies that 10 (decimal) is used, but not all browsers support this yet. For this reason always specify a radix when using parseInt.
If the input string begins with any other value, the radix is 10 (decimal).


所以这里radix值实际为10，所以结果为1

而parseInt("2", 1)和parseInt("3", 2)则确实无法解析，会生成NaN

所以答案为[1,NaN,NaN]，为D

第二题和第五题

What is the result of this expression? (or multiple ones)

```
[typeof null, null instanceof Object]
```

A: ["object", false]

B: [null, false]

C: ["object", true]

D: other

考察typeof运算符和instanceof运算符，上MDN上看一下typeof运算符，一些基础类型的结果为：
Undefined "undefined" 

Null "object"  

Boolean "boolean" 

Number "number"

String "string"

Any other object "object"

Array "object"

自从javascript创造出来，typeof null的值就是object了(这是个历史问题)

而null instanceof 任何类型 都是false

所以答案为["object", false], 选A

第三题

What is the result of this expression? (or multiple ones)

```
[ [3,2,1].reduce(Math.pow), [].reduce(Math.pow)] ]
```

A: an error

B: [9, 0]

C: [9, NaN]

D: [9, undefined]

这题考的Math.pow和Array.prototype.reduce

Math.pow(base, exponent)接受两个参数：基数、需要计算的次方

reduce传递给其作为参数的函数几个值：

previousValue：上一次计算的结果

currentValue：当前元素的值

index： 当前元素在数组中的位置

rray：整个数组


reduce本身接受两个参数，callback和initialValue，分别是reduce的回调函数和计算初始值--也就是第一次reduce的callback被调用时的previousValue的值，默认为0

reduce在数组为空且没有定义initialValue时，会抛出错误，如chrome下：TypeError: Reduce of empty array with no initial value

所以选A


第四题

What is the result of this expression? (or multiple ones)

```
var val = 'smtg';
console.log('Value is ' + (val === 'smtg') ?'Something' : 'Nothing');
```
A: Value is Something

B: Value is Nothing

C: NaN

D: other

这题考的javascript中的运算符优先级，MDN传送门，这里'+'运算符的优先级要高于'?'所以运算符，实际上是 'Value is true'?'Something' : 'Nothing'，当字符串不为空时，转换为bool为true，所以结果为'Something'，选D

第六题

What is the result of this expression? (or multiple ones)

```
var name = 'World!';
(function () {
    if (typeof name === 'undefined') {
        var name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
})();
```
A: Goodbye Jack

B: Hello Jack

C: Hello undefined

D: Hello World

这题考的是javascript作用域中的变量提升，javascript的作用于中使用var定义的变量都会被提升到所有代码的最前面，于是乎这段代码就成了：

```
var name = 'World!';
(function () {
    var name;//现在还是undefined
    if (typeof name === 'undefined') {
        name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
})();
```
这样就很好理解了，typeof name === 'undefined'的结果为true，所以最后会输出'Goodbye Jack'，选A


第七题

What is the result of this expression? (or multiple ones)

```
var END = Math.pow(2, 53);
var START = END - 100;
var count = 0;
for (var i = START; i <= END; i++) {
    count++;
}
console.log(count);
```
A: 0

B: 100

C: 101

D: other

这题考查javascript中的数字的概念：首先明确一点，javascript和其他语言不同，仅有一种数字，IEEE 754标准的64位浮点数，能够表示的整数范围是-2^53~2^53（包含边界值），所以Math.pow(2, 53)即为javascript中所能表示的最大整数，在最大整数在继续增大就会出现精度丢失的情况，END + 1 的值其实是等于END的，这也就造成了死循环，所以选D

第八题

What is the result of this expression? (or multiple ones)

```
var ary = [0,1,2];
ary[10] = 10;
ary.filter(function(x) { return x === undefined;});
```

A: [undefined × 7]

B: [0, 1, 2, 10]

C: []

D: [undefined]

考查Array.prototype.filter方法的使用，MDN上有这么一句it is not invoked for indexes which have been deleted or which have never been assigned values，所以结果为空数组，选C

第九题

What is the result of this expression? (or multiple ones)

```
var two   = 0.2
var one   = 0.1
var eight = 0.8
var six   = 0.6
[two - one == one, eight - six == two]
```
A: [true, true]

B: [false, false]

C: [true, false]

D: other

浮点数计算时的精度丢失问题，其他语言也会出现...至于结果，反正我是蒙的...chrome中计算出来的结果：[0.1, 0.20000000000000007]，也就是[true, false]，选C

第十题

What is the result of this expression? (or multiple ones)

```
function showCase(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase(new String('A'));
```
A: Case A

B: Case B

C: Do not know!

D: undefined

这题考的是使用new方法创建基础类型，使用new方法创建的基础类型，首先来看个栗子(chrome):

```
typeof new String("skyinlayer");
"object"
typeof "skyinlayer";
"string"
```
这样基本上就能看到结果了，但是为什么呢？MDN上的解释是，字符串字面量和直接调用String()方法（不使用new调用构造函数）的结果是原始字符串。JS自动回转化原始字符串到String对象。所以可以在原始字符串上使用用String对象的方法。而在上下文中，在原始字符串的方法被调用或者从其中获取属性时，JS会自动包裹原始字符串然后调用方法或者获取属性。

所以呢，JS本身有原始字符串和字符串对象之分，只不过在调用方法和获取属性时的时候会自动转换，但typeof运算符运算时是不会转换的。Number和Boolean同样适用

所以这里结果为Do not know!，选C

第十一题

What is the result of this expression? (or multiple ones)

```
function showCase2(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase(String('A'));
```
A: Case A

B: Case B

C: Do not know!

D: undefined

和上题原理一样，不过这里没有使用new来生成字符串，所以生成的结果就是原始字符串，相当于showCase('A')，所以结果就是A了

第十二题

What is the result of this expression? (or multiple ones)

```
function isOdd(num) {
    return num % 2 == 1;
}
function isEven(num) {
    return num % 2 == 0;
}
function isSane(num) {
    return isEven(num) || isOdd(num);
}
var values = [7, 4, '13', -9, Infinity];
values.map(isSane);
```
A: [true, true, true, true, true]

B: [true, true, true, true, false]

C: [true, true, true, false, false]

D: [true, true, false, false, false]

还是JS的数字相关，不过这次考察的是取模，这题我也是瞎蒙的（果断跪了）。

前两个基本上没什么疑问，必然是true

'13'在进行计算前则会进行隐式类型转换（JS最恶心的部分之一），详细参见$雨$的文章《Javascript类型转换的规则》，这里的规则就是将字符串通过Number()方法转换为数字，所以结果为13 % 2 ，也就是true

而JS中负数取模的结果是负数，这里-9%2的结果实际上是-1，所以为false

而Infinity对任意数取模都是NaN，所以是false

综上，结果为[true, true, true, false, false]，也就是C

第十三题

What is the result of this expression? (or multiple ones)

```
parseInt(3, 8)
parseInt(3, 2)
parseInt(3, 0)
```
A: 3, 3, 3

B: 3, 3, NaN

C: 3, NaN, NaN

D: other

还是parseInt的题，考的和第一题类似，第一个值为3没什么好说的。如果出现的数字不符合后面输入的进制，则为NaN，所以第二个值为NaN。而radix为0时的情况第一题下面有介绍，这里也是一样为默认10，所以结果为3，所以答案为3, NaN, 3，选D

第十四题

What is the result of this expression? (or multiple ones)

```
Array.isArray( Array.prototype )
```

A: true

B: false

C: error

D: other

死知识，MDN传送门，这是MDN官方给的例子...

第十五题

What is the result of this expression? (or multiple ones)

```
var a = [0];
if ([0]) { 
  console.log(a == true);
} else { 
  console.log("wut");
}
```
A: true

B: false

C: "wut"

D: other

同样是一道隐式类型转换的题，不过这次考虑的是'=='运算符，a本身是一个长度为1的数组，而当数组不为空时，其转换成bool值为true。

而==左右的转换，会使用如果一个操作值为布尔值，则在比较之前先将其转换为数值的规则来转换，Number([0])，也就是0，于是变成了0 == true，结果自然是false，所以最终结果为B

第十六题

What is the result of this expression? (or multiple ones)

```
[] == []
```
A: true

B: false

C: error

D: other

这题考的是数组字面量创建数组的原理和==运算符，首先JS中数组的真实类型是Object这点很明显typeof []的值为"object"，而==运算符当左右都是对象时，则会比较其是否指向同一个对象。而每次调用字面量创建，都会创造新的对象，也就是会开辟新的内存区域。所以指针的值自然不一样，结果为 false，选B

第十七题

What is the result of this expression? (or multiple ones)

```
'5' + 3  
'5' - 3  
```
A: 53, 2

B: 8, 2

C: error

D: other

又是一道隐式类型转换的题

加法： 加法运算中，如果有一个操作值为字符串类型，则将另一个操作值转换为字符串，最后连接起来

减法： 如果操作值之一不是数值，则被隐式调用Number()函数进行转换

所以第一行结果为字符串运算，为'53'。第二行结果为2，选A

第十八题

What is the result of this expression? (or multiple ones)

```
1 + - + + + - + 1 
```
A: 2

B: 1

C: error

D: other

C语言中的经典...对于这种问题，原理什么的不懂，蒙吧，结果是2

第十九题

What is the result of this expression? (or multiple ones)

```
var ary = Array(3);
ary[0]=2
ary.map(function(elem) { return '1'; }); 
```
A: [2, 1, 1]

B: ["1", "1", "1"]

C: [2, "1", "1"]

D: other

又是考的Array.prototype.map的用法，map在使用的时候，只有数组中被初始化过元素才会被触发，其他都是undefined，所以结果为["1", undefined × 2]，选D

第二十题

What is the result of this expression? (or multiple ones)

```
function sidEffecting(ary) { 
  ary[0] = ary[2];
}
function bar(a,b,c) { 
  c = 10
  sidEffecting(arguments);
  return a + b + c;
}
bar(1,1,1)
```
A: 3

B: 12

C: error

D: other

这题考的是JS的函数arguments的概念：

在调用函数时，函数内部的arguments维护着传递到这个函数的参数列表。它看起来是一个数组，但实际上它只是一个有length属性的Object，不从Array.prototype继承。所以无法使用一些Array.prototype的方法。

arguments对象其内部属性以及函数形参创建getter和setter方法，因此改变形参的值会影响到arguments对象的值，反过来也是一样.

第二十一题

What is the result of this expression? (or multiple ones)

```
var a = 111111111111111110000,
    b = 1111;
a + b;
```
A: 111111111111111111111

B: 111111111111111110000

C: NaN

D: Infinity

又是一道考查JavaScript数字的题，与第七题考察点相似。由于JavaScript实际上只有一种数字形式IEEE 754标准的64位双精度浮点数，其所能表示的整数范围为-2^53~2^53(包括边界值)。这里的111111111111111110000已经超过了2^53次方，所以会发生精度丢失的情况。综上选B

第二十二题

What is the result of this expression? (or multiple ones)

```
var x = [].reverse;
x();
```
A: []

B: undefined

C: error

D: window

这题考查的是函数调用时的this和Array.prototype.reverse方法。

首先看Array.prototype.reverse方法，首先举几个栗子：

```
console.log(Array.prototype.reverse.call("skyinlayer"));
//skyinlayer
console.log(Array.prototype.reverse.call({}));
//Object {}
console.log(Array.prototype.reverse.call(123));
//123
```
这几个栗子可以得出一个结论，Array.prototype.reverse方法的返回值，就是this

Javascript中this有如下几种情况：

全局下this，指向window对象

```
console.log(this);
//输出结果：
//Window {top: Window, window: Window, location: Location, external: Object, chrome: Object…}
函数调用，this指向全局window对象：

function somefun(){
    console.log(this);
}
somefun();
//输出结果：
//Window {top: Window, window: Window, location: Location, external: Object, chrome: Object…}
方法调用，this指向拥有该方法的对象：

var someobj = {};
someobj.fun = function(){
    console.log(this);
};
console.log(someobj.fun());
//输出结果：
//Object {fun: function}

调用构造函数，构造函数内部的this指向新创建对象：

function Con() {
    console.log(this);
}
Con.prototype.somefun = function(){};
console.log(new Con());
//输出结果：
//Con {somefun: function}
显示确定this：

function somefun(){
    console.log(this);
};
somefun.apply("skyinlayer");
somefun.call("skyinlayer");
//输出结果：
//String {0: "s", 1: "k", 2: "y", 3: "i", 4: "n", 5: "l", 6: "a", 7: "y", 8: "e", 9: "r", length: 10}
//String {0: "s", 1: "k", 2: "y", 3: "i", 4: "n", 5: "l", 6: "a", 7: "y", 8: "e", 9: "r", length: 10} 
```
这里可以看到，使用的是函数调用方式，this指向的是全局对象window，所以选D

第二十三题

What is the result of this expression? (or multiple ones)

```
Number.MIN_VALUE > 0
```
A: false

B: true

C: error

D: other

考查的Number.MIN_VALUE的概念，MDN传送门，关键的几句话
* The Number.MIN_VALUE property represents the smallest positive numeric value representable in JavaScript.
翻译：Number.MIN_VALUE表示的是JavaScript中最小的正数

The MIN_VALUE property is the number closest to 0, not the most negative number, that JavaScript can represent.
翻译：MIN_VALUE是接近0的数，而不是最小的数

MIN_VALUE has a value of approximately 5e-324. Values smaller than MIN_VALUE ("underflow values") are converted to 0.
翻译：MIN_VALUE值约等于5e-324，比起更小的值（大于0），将被转换为0

所以，这里是true，选B

顺带把Number的几个常量拉出来：

* Number.MAX_VALUE：最大的正数
* Number.MIN_VALUE：最小的正数
* Number.NaN：特殊值，用来表示这不是一个数
* Number.NEGATIVE_INFINITY：负无穷大
* Number.POSITIVE_INFINITY：正无穷大

如果要表示最小的负数和最大的负数，可以使用-Number.MAX_VALUE和-Number.MIN_VALUE

第二十四题

What is the result of this expression? (or multiple ones)

```
[1 < 2 < 3, 3 < 2 < 1]
```
A: [true, true]

B: [true, false]

C: error

D: other

运算符的运算顺序和隐式类型转换的题，从MDN上运算符优先级，'<'运算符顺序是从左到右，所以变成了[true < 3, false < 1]

接着进行隐式类型转换，'<'操作符的转换规则（来自$雨$的文章《Javascript类型转换的规则》）:

如果两个操作值都是数值，则进行数值比较
如果两个操作值都是字符串，则比较字符串对应的字符编码值
如果只有一个操作值是数值，则将另一个操作值转换为数值，进行数值比较
如果一个操作数是对象，则调用valueOf()方法（如果对象没有valueOf()方法则调用toString()方法），得到的结果按照前面的规则执行比较
如果一个操作值是布尔值，则将其转换为数值，再进行比较
所以，这里首先通过Number()转换为数字然后进行比较，true会转换成1，而false转换成0，就变成了[1 < 3, 0 < 1]

所以结果为A

第二十五题

What is the result of this expression? (or multiple ones)

```
// the most classic wtf
2 == [[[2]]]
```
A: true

B: false

C: undefined

D: other

又是隐式类型转换的题（汗）

题目作者的解释是：
both objects get converted to strings and in both cases the resulting string is "2"

也就是说左右两边都被转换成了字符串，而字符串都是"2"

这里首先需要对==右边的数组进行类型转换，根据以下规则（来自justjavac的文章《「译」JavaScript 的怪癖 1：隐式类型转换》）：

1. 调用 valueOf()。如果结果是原始值（不是一个对象），则将其转换为一个数字。

2. 否则，调用 toString() 方法。如果结果是原始值，则将其转换为一个数字。

3. 否则，抛出一个类型错误。

所以右侧被使用toString()方法转换为"2"，然后又通过Number("2")转换为数字2进行比较，结果就是true了，选A

第二十六题

What is the result of this expression? (or multiple ones)

```
3.toString()
3..toString()
3...toString()
```
A: "3", error, error

B: "3", "3.0", error

C: error, "3", error

D: other

说实话这题有点常见了，很多人都踩过3.toString()的坑（包括我）...虽然JavaScript会在调用方法时对原始值进行包装，但是这个点是小数点呢、还是方法调用的点呢，于是乎第一个就是error了，因为JavaScript解释器会将其认为是小数点。

而第二个则很好说通了，第一个点解释为小数点，变成了(3.0).toString()，结果就是"3"了

第三个也是，第一个点为小数点，第二个是方法调用的点，但是后面接的不是一个合法的方法名，于是乎就error了

综上，选C

第二十七题

What is the result of this expression? (or multiple ones)

```
(function(){
  var x = y = 1;
})();
console.log(y);
console.log(x);
```
A: 1, 1

B: error, error

C: 1, error

D: other

变量提升和隐式定义全局变量的题，也是一个JavaScript经典的坑...

还是那句话，在作用域内，变量定义和函数定义会先行提升，所以里面就变成了:

```
(function(){
    var x;
    y = 1;
    x = 1;
})();
```
这点会问了，为什么不是var x, y;，这就是坑的地方...这里只会定义第一个变量x，而y则会通过不使用var的方式直接使用，于是乎就隐式定义了一个全局变量y

所以，y是全局作用域下，而x则是在函数内部，结果就为1, error，选C

第二十八题

What is the result of this expression? (or multiple ones)

```
var a = /123/,
    b = /123/;
a == b
a === b
```
A: true, true

B: true, false

C: false, false

D: other

首先需要明确JavaScript的正则表达式是什么。JavaScript中的正则表达式依旧是对象，使用typeof运算符就能得出结果：

console.log(typeof /123/);
//输出结果：
//"object"
==运算符左右两边都是对象时，会比较他们是否指向同一个对象，可以理解为C语言中两个指针的值是否一样（指向同一片内存），所以两个结果自然都是false

第二十九题

What is the result of this expression? (or multiple ones)

```
var a = [1, 2, 3],
    b = [1, 2, 3],
    c = [1, 2, 4]
a == b
a === b
a > c
a < c
```
A: false, false, false, true

B: false, false, false, false

C: true, true, false, true

D: other

和上题类似，JavaScript中Array的本质也是对象，所以前两个的结果都是false，

而JavaScript中Array的'>'运算符和'<'运算符的比较方式类似于字符串比较字典序，会从第一个元素开始进行比较，如果一样比较第二个，还一样就比较第三个，如此类推，所以第三个结果为false，第四个为true。

综上所述，结果为false, false, false, true，选A

第三十题

What is the result of this expression? (or multiple ones)

```
var a = {}, b = Object.prototype;
[a.prototype === b, Object.getPrototypeOf(a) === b]
```
A: [false, true]

B: [true, true]

C: [false, false]

D: other

原型链的题（总会有的），考查的__proto__和prototype的区别。首先要明确对象和构造函数的关系，对象在创建的时候，其__proto__会指向其构造函数的prototype属性

Object实际上是一个构造函数（typeof Object的结果为"function"）,使用字面量创建对象和new Object创建对象是一样的，所以a.__proto__也就是Object.prototype，而Object.getPrototypeOf(a)与a.__proto__是一样的，所以第二个结果为true

而实例对象是没有prototype属性的，只有函数才有，所以a.prototype其实是undefined，第一个结果为false

综上，选A

第三十一题

What is the result of this expression? (or multiple ones)

```
function f() {}
var a = f.prototype, b = Object.getPrototypeOf(f);
a === b
```
A: true

B: false

C: null

D: other

还是__proto__和prototype的区别，两者不是一个东西，所以选B

第三十二题

What is the result of this expression? (or multiple ones)

```
function foo() { }
var oldName = foo.name;
foo.name = "bar";
[oldName, foo.name]
```
A: error

B: ["", ""]

C: ["foo", "foo"]

D: ["foo", "bar"]

考察了函数的name属性，使用函数定义方式时，会给function对象本身添加一个name属性，保存了函数的名称，很好理解oldName为"foo"。name属性时只读的，不允许修改，所以foo.name = "bar";之后，foo.name还是"foo"，所以结果为["foo", "foo"]，选C

PS：name属性不是一个标准属性，不要去使用，除非你想要坑别人

第三十三题

What is the result of this expression? (or multiple ones)

```
"1 2 3".replace(/\d/g, parseInt)
```
A: "1 2 3"

B: "0 1 2"

C: "NaN 2 3"

D: "1 NaN 3"

String.prototype.replace、正则表达式的全局匹配和parseInt（又是parseInt...），可以根据题意看出来题目上漏了一个'\'

首先需要确定replace会传给parseInt哪些参数。举个栗子:

```
"1 2 3".replace(/\d/g, function(){
    console.log(arguments);
});
//输出结果：
//["1", 0, "1 2 3"]
//["2", 2, "1 2 3"]
//["3", 4, "1 2 3"] 
```
一共三个：

1. match：正则表达式被匹配到的子字符串

2. offset：被匹配到的子字符串在原字符串中的位置

3. string：原字符串

这样就很好理解了，又回到之前parseInt的问题了，结果就是parseInt("1", 10), parseInt("2", 2), parseInt("3", 4)所以结果为"1, NaN, 3"，选D

第三十四题

What is the result of this expression? (or multiple ones)

```
function f() {}
var parent = Object.getPrototypeOf(f);
f.name // ?
parent.name // ?
typeof eval(f.name) // ?
typeof eval(parent.name) //  ?
```
A: "f", "Empty", "function", "function"

B: "f", undefined, "function", error

C: "f", "Empty", "function", error

D: other

又是Function.name属性的题，和三十二题一样样，f.name值为"f"，而eval("f")则会输出f函数，所以结果为"function"

接着看parent，parent实际上就是f.__proto__，需要明确的是JavaScript中的函数也是对象，其也有自己的构造函数Function，所以f.__proto__ === Function.prototype结果是true，而Function.prototype就是一个名为Empty的function

console.log(Function.prototype);
console.log(Function.prototype.name);
//输出结果：
//function Empty() {}
//Empty
所以parent.name的值为Empty

如果想直接在全局作用域下调用Empty，显示未定义...因为Empty并不在全局作用域下

综上所述，结果为C

第三十五题

What is the result of this expression? (or multiple ones)

```
var lowerCaseOnly =  /^[a-z]+$/;
[lowerCaseOnly.test(null), lowerCaseOnly.test()]
```
A: [true, false]

B: error

C: [true, true]

D: [false, true]

正则表达式的test方法会自动将参数转换为字符串，原式就变成了`[lowerCaseOnly.test("null"),lowerCaseOnly.test("undefined")]`，结果都是真，所以选C

第三十六题

What is the result of this expression? (or multiple ones)

```
[,,,].join(", ")
```
A: ", , , "

B: "undefined, undefined, undefined, undefined"

C: ", , "

D: ""

JavaScript中使用字面量创建数组时，如果最末尾有一个逗号','，会背省略，所以实际上这个数组只有三个元素（都是undefined）：

```
console.log([,,,].length);
//输出结果：
//3
```
而三个元素，使用join方法，只需要添加两次，所以结果为", , "，选C

第三十七题

What is the result of this expression? (or multiple ones)

```
var a = {class: "Animal", name: 'Fido'};
a.class
```
A: "Animal"

B: Object

C: an error

D: other

经典坑中的一个，class是关键字。根据浏览器的不同，结果不同：

chrome的结果： "Animal"

Firefox的结果："Animal"

Opera的结果："Animal"

IE 8以上也是： "Animal"

IE 8 及以下： 报错

第三十八题

What is the result of this expression? (or multiple ones)

```        
var a = new Date("epoch")
```
A:Thu Jan 01 1970 01:00:00 GMT+0100 (CET)

B:current time

C:error

D:other

选 D ，正确答案为 Invalid Date 。Date虽然是一个对象，但是并不能返回正确的时间，因为时间是以数字字符串的形式存在的，所以要进行字符串转换后才会反水具体的时间。

第三十九题

What is the result of this expression? (or multiple ones)

```          
var a = Function.length,
    b = new Function().length
a === b
```
A: true
 
B: false
 
C: error
 
D: other     

这个答案选 B 。因为 Function的长度是从 1 开始算的，而 new Function 的 长度是从 0 开始算的。

第四十题 

What is the result of this expression? (or multiple ones)

```          
var a = Date(0);
var b = new Date(0);
var c = new Date();
[a === b, b === c, a === c]
```
A: [true, true, true, true]

B: [false, false, false] 

C: [false, true, false]

D: [true, false, false]

答案选 B 。

第四十一题 

What is the result of this expression? (or multiple ones)

```          
var min = Math.min(), max = Math.max()
min < max
```
A: true 

B: false

C:error 

D: other 

答案选 B false. 因为 Math.min 为空的时候返回的是正无穷，而Math.max为空的时候返回的是负无穷。

第四十二题

What is the result of this expression? (or multiple ones)

```         
function captureOne(re, str) {
  var match = re.exec(str);
  return match && match[1];
}
var numRe  = /num=(\d+)/ig,
    wordRe = /word=(\w+)/i,
    a1 = captureOne(numRe,  "num=1"),
    a2 = captureOne(wordRe, "word=1"),
    a3 = captureOne(numRe,  "NUM=2"),
    a4 = captureOne(wordRe,  "WORD=2");
[a1 === a2, a3 === a4]
```
A: [true, true]

B: [false, false]

C: [true, false]

D: [false, true]

答案选 C 





---
layout: post
title: attribute 和 property 的区别
date: 2015-07-05 11:21:04
tags: JavaScript
---


之前很早用 jQuery 的时候就遇到这个问题，想写篇文章来研究一下这两个的区别。但是只在印象笔记里开了个头就没继续了。刚好前段时间看到 winter 在微博上写了篇短文，我就直接参考啦。

一、概述

attribute和property是常常被弄混的两个概念。

简单来说，property则是JS代码里访问的：

    document.getElementByTagName('my-element').prop1 = 'hello';
attribute类似这种：

    <my-element attr1="cool" />
JS代码里访问attribute的方式是getAttribute和setAttribute：

    document.getElementByTagName('my-element').setAttribute('attr1','Hello');
    document.getElementByTagName('my-element').getAttribute('attr1','Hello');

二、区别

多数情况下，两者是等效的。在web标准中，常常会规定某attribute“反射”了同名的property。但是例外的情况还是不少的。

1. 名字不一致

最典型的是className，为了回避JavaScript保留字，JS中跟class attribute对应的property是className。

    <div class="cls1 cls2"></div>
    <script>
        var div = document.getElementByTagName('div');
        div.className //cls1 cls2
    </scrpit>
2. 类型不一致

最典型的是style，不接受字符串型赋值。

    <div class="cls1 cls2" style="color:blue" ></div>
    <script>
        var div = document.getElementByTagName('div');
        div.style // 对象
    </scrpit>
3. 语义不一致

如a元素的href属性。

    <a href="//m.taobao.com" ></div>
    <script>
        var a = document.getElementByTagName('a');
        a.href // “http://m.taobao.com”，这个url是resolve过的结果
        a.getAttribute('href') //  “//m.taobao.com”，跟HTML代码中完全一致
    </scrpit>
4. 单向同步关系

value是一个极为特殊的attribute/property。

    <input value = "cute" />
    <script>
        var input = document.getElementByTagName('input');
        //若property没有设置，则结果是attribute
        input.value //cute
        input.getAttribute('value'); //cute

        input.value = 'hello';
        //若value属性已经设置，则attribute不变，property变化，元素上实际的效果是property优先
        input.value //hello
        input.getAttribute('value'); //cute
    </scrpit>
除此之外，checkbox的显示状态由checked和indeterminate两个property决定，而只有一个名为checked的property，这种情况下property是更完善的访问模型。

三、特殊场景

1.mutation

使用mutation observer，只能监测到attribute变化。

  var observer = new MutationObserver(function(mutations){
      for(var i = 0; i < mutations.length; i++) {
          var mutation = mutations[i];
          console.log(mutation.attributeName);
      }
  });
  observer.observe(element,{attributes:true});
  element.prop1 = 'aa' // 不会触发
  element.setAttribute('attr1', 'aa') //会触发
2.custom element

在使用WebComponents时，可以定义attribute和property，两者可以互相反射，也可以全无关联。

    var MyElementProto = Object.create(HTMLElement.prototype, {
        createdCallback : { 
            value : function() {  }
        }
    });

    //定义property
    Object.defineProperty(MyElementProto,'prop1', {
        get:function(){
            return //
        },
        set:function(){
            console.log('property change');//do something
        }
    });

    //定义attribute
    MyElementProto.attributeChangedCallback = function(attr, oldVal, newVal) {
        if(attr === 'attr1') {
            console.log('attribute change');//do something
        }
    };

    window.MyElement = document.registerElement('my-element', {
        prototype: MyElementProto
    });




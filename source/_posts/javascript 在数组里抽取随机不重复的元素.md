---
layout: post
title: javascript 在数组里抽取随机不重复的元素
date: 2015-07-26 11:21:04
tags: JavaScript
---

我们在业务里可能经常有这养的需求，在页面上随机显示几个不重复的东西，还有肯能随机抽奖的时候也需要用到。所以今天就总结一下这个用法。
方法一：
这个是我在项目里写的方法，
<!-- more -->
```
    /**
     * 从数组里随机取几个不重复的元素
     * @param  {Array} arr [description]
     * @param  {Number} num [description]
     * @return {[type]}     返回新的数组
     */
     
    function getArrayItems (arr, num) {
        //新建一个数组,将传入的数组复制过来,用于运算,而不直接操作传入的数组;
        var copyArr = [];
        for (var index in arr) {
            copyArr.push(arr[index]);
        }
        //取出的数值项,保存在此数组返回
        var resultArr = [];
        for (var i = 0; i<num; i++) {
            //判断如果数组还有可以取出的元素,以防下标越界
            if (copyArr.length>0) {
                //在数组中产生一个随机索引
                var arrIndex = Math.floor(Math.random()*copyArr.length);
                //将此随机索引的对应的数组元素值复制出来
                resultArr[i] = copyArr[arrIndex];
                //然后删掉此索引的数组元素,这时候 copyArr 变为新的数组
                copyArr.splice(arrIndex, 1);
            } else {
                //数组中数据项取完后,退出循环,比如数组本来只有10项,但要求取出20项.
                break;
            }
        }
        return resultArr;
    }
```

然后我用的时候，比如后端传给我的是一系列视频 Arr，我需要随机在页面显示 3 个，就这样调用 

```
getArrayItems (Arr, 3)
```

在这里有的人可能是直接操作传入的数组，一般也不会有什么问题，但是为了谨慎起见，还是操作一个新的数组.

方法二：

```
function getArrayItems ( arr, num ){
	 var copyArr = []
	 for (var index in arr) {
        copyArr.push(arr[index]);
   }
    copyArr.sort(function(){
        return Math.random() - 0.5;
    });
    // 从原数组中一次性返回需要的元素
    return copyArr.slice( 0,  num );
}
```

调用： ```getArrayItems(arr,3)```

第二个方法代码比第一个方法还简单，为什么我会用第一个方法呢，因为第二个方法看似简单，但是却也很暴力，只要数组长度一长性能损耗更大。


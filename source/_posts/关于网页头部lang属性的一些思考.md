---
layout: post
title: 关于网页头部lang属性的一些思考
date: 2013-12-31 11:21:04
tags: JavaScript
---

`<html lang="zh-CN">`;我相信很多人都熟悉这个东西，这个就是我们平常写网页头部的那个语言声明吧，估计很多人和我一样以为默认的中文就是这么写，但是今天在知乎看到一个问题，发现我们了解的真是不够啊...

![](http://m2.img.libdd.com/farm5/d/2013/1231/20/28ADBA93FBD527757EA11C66ADA55C22_B500_900_500_75.png)

首先lang属性的取值应该遵循 <a href="http://tools.ietf.org/html/bcp47" target="_blank" rel="nofollow">BCP 47 - Tags for Identifying Languages<i></i></a>。而单一的 zh 和 zh-CN 均属于废弃用法。因为问题主要在于，zh 现在不是语言code了，而是macrolang，能作为语言code的是cmn（国语）、yue（粤语）、wuu（吴语）等。我通常建议写成 zh-cmn 而不是光写 cmn，主要是考虑兼容性（至少可匹配 zh），有不少软件和框架还没有据此更新。

zh-CN 的问题还在于，其实多数情况下标记的是简体中文，但是不恰当的使用了地区，这导致同样用简体中文的 zh-SG（新加坡）等无法匹配。更典型的是 zh-TW 和 zh-HK。所以其实应该使用 zh-Hans / zh-Hant 来表示简体和繁体。那么完整的写法就是 zh-cmn-Hans，表示简体中文书写的普通话/国语。一般而言没有必要加地区代码，除非要表示地区特异性，一般是词汇不一样（比如维基百科的大陆简体和新马简体）。

但是，由于历史原因，有时候不得不继续使用zh-CN。比如中文维基百科，沿用了传统的zh-CN/zh-HK/zh-SG/zh-TW（按照标准应该使用 zh-cmn-Hans-CN、zh-cmn-Hant-HK、zh-cmn-Hans-SG、zh-cmn-Hant-TW）。这时候，合理的软件行为，是将 zh-CN 等转化为 zh-cmn-Hans（即转化为最常见的误用所对应的实际标准写法）。

最后，所以说要真正正确的写应该是按这个步骤来的 lang= cmn，但是为了兼容性可以写成lang=zh-cmn或者zh-cmn-Hans亦或者lang="zh-cmn-Hans-CN"

当然继续用zh-CN也没多大的问题，如果你不知道怎么写不写lang也可以，嘿嘿...纯属探讨思考，求高人指点。




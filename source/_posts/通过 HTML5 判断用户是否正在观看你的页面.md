---
layout: post
title:  通过 HTML5 判断用户是否正在观看你的页面
date: 2015-09-11 11:21:04
tags: JavaScript
---

今天登录饿了么网页版（平常一般只用手机）发现一个现象，就是当我打开他们页面的时候，网页浏览器地址栏里的 title 显示的是 饿了么，但是当我切换另一个窗口页面的时候，发现饿了么那边的 title 变成了「记得回来吃饭哦！」。觉得有点好奇，就查了下这个怎么实现。其实很简单的，是 HTML5  里的一个新特性。

HTML5 里在页面可见性接口提供给了程序员一个方法，让他们使用 visibilitychange 页面事件来判断当前页面可见性的状态，并针对性的执行某些任务。同时还有新的 document.hidden 属性可以使用。监听，当 document.hidden 为 true 时说明当前页面不可见，这时候你可以替换你的网页 title ，还可以操作一些其他的事情，比如当用户正在看视频的时候，切换到另一个页面的话，此时你监听的页面 document.hidden 为 true ，然后还可以让视频停止播放。
 
而 document.visibilityState ，visibilityState 的值要么是 visible (表明页面为浏览器当前激活tab，而且窗口不是最小化状态)，要么是hidden (页面不是当前激活tab页面，或者窗口最小化了。)，或者prerender (页面在重新生成，对用户不可见。)。

下面我们可以写一个事件来监听页面的变化：

```
// 各种浏览器兼容
var hidden, state, visibilityChange; 
if (typeof document.hidden !== "undefined") {
	hidden = "hidden";
	visibilityChange = "visibilitychange";
	state = "visibilityState";
} else if (typeof document.mozHidden !== "undefined") {
	hidden = "mozHidden";
	visibilityChange = "mozvisibilitychange";
	state = "mozVisibilityState";
} else if (typeof document.msHidden !== "undefined") {
	hidden = "msHidden";
	visibilityChange = "msvisibilitychange";
	state = "msVisibilityState";
} else if (typeof document.webkitHidden !== "undefined") {
	hidden = "webkitHidden";
	visibilityChange = "webkitvisibilitychange";
	state = "webkitVisibilityState";
}

// 添加监听器，在title里显示状态变化
document.addEventListener(visibilityChange, function() {
	if (document[state] === 'hidden') {
		document.title = '记得回来吃饭'; // 页面不可见时 ，可换成你的 title
	} else {
		document.title = '饿了么'; // 页面可见
	}
	
}, false);

// 初始化
document.title = '';
```
仔细想想这个功能还是有点用处的，具体的 效果可参见我的一个 DEMO: http://blog.yongyuan.us/shengchen



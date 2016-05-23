---
layout: post
title: 纯 CSS 制作的居中弹出层
date: 2014-09-13 11:21:04
tags: css
---


刚刚翻了下大漠的那本书看到那个 CSS3 的 target 属性我也是醉了，用的活这个可以用到很多地方。你想了解更多 CSS3 的技巧还是去看看大漠的那本书吧，才 9.9人民币。真想不到那些写书的大神怎么想的，辛辛苦几年写的书，几块钱就答应卖出去，想想就醉了...

主要用到的知识点: CSS3里的 target 选择器。其余的自行发挥...

ps:刚刚网上搜了一圈，那些所谓的纯 css 制作的 lightbox 效果都是假的，都用到了或多或少的 js 事件。

直接上代码结构了:

    
    HTML:
    	<section>
    	 <a href="#alert" class="button">
    <img src="http://img.t.sinajs.cn/t4/appstyle/expression/ext/normal/b6/doge_org.gif"  /></a>
    	</section>
    	<div class="box-target" id="alert">
    	  <img src="http://tp3.sinaimg.cn/2175915602/180/5705233498/1">
    	  <a class="box-close" href="#"></a>
    	</div>


SCSS:
	
	/**
	By Go7hic
	jsfiddle 上面的 sass 不带 Compass 也是醉了，所以几个css3属性的混合宏不能直接调用，还是直接用css写出来吧。
	**/
	body {
	  background: #333;
	  font-size: 1em;
	  color: #f0f0f0;
	  line-height: 1.1em;
	}
	a {
	  text-decoration: none;
	  color: #313131;
	  &:hover {
	    color: #000;
	  }
	}
	section {
	  width: 500px;
	  margin: 0 auto;
	  background: #555;
	  padding: 1em;
	}
	
	a.button {
		display: inline-block;
		background: #999;
		color: #f2f2f2;
		border-radius: 50%; /* 直接写啦,后面的也是 */
		padding: 10px 1em;
	    box-shadow: 0 3px 0 #777; 
		font-size: 1.3em;
		text-decoration: none;
	}
	
	.box-target {
		position: fixed;
		top: -100%;
		width: 100%;
		background: rgba(0,0,0,.7);
		opacity: 0;
		transition: opacity .5s ease-in-out;
		overflow: hidden;
	     img {
	        margin: auto;
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			max-height: 0%;
			max-width: 0%;
			border: 3px solid #fff;
			box-shadow: 0 0 8px rgba(0,0,0,.3);
			box-sizing: border-box;
		    transition: .5s ease-in-out;
	     }
		&:target {
			opacity: 1;
			top: 0;
			bottom: 0;
	        img {
	            max-height: 100%;
	            max-width: 100%;
	        }
			.box-close {
				top: 5%;
			}
		}
	}
	
	.box-close {
		display: block;
		width: 50px;
		height: 50px;
		box-sizing: border-box;
		background: #139dd7;
		color: #fff;
		position: absolute;
		top: 10%;
		right: 5%;
		transition: .5s ease-in-out;
	    &:before, &:after {
			content: " ";
			display: block;
			height: 30px;
			width: 1px;
			background: #fff;
			position: absolute;
			left: 26px;
			top: 10px;
			transform: rotate(45deg);
		}
		&:after {
			transform: rotate(-45deg);
		}
	}


[DEMO](http://jsfiddle.net/gothic/8wbo4jf5/19/embedded/result/)
这个应用场景应该有很多地方，如果你的团队无视 IE 浏览器，推行 HTML5 和 CSS3，那么可以试试这个方法。




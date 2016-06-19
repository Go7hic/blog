---
layout: post
title: 你真的了解HTML头部标签吗？
date: 2014-05-08 11:21:04
tags: HTML5
---
习惯了Emmet这样的工具，我们几乎没怎么写过html文档头部标签了，今天这篇文章其实就是总结了一下当下各个设备的头部标签的写法。

基本标签

使用 HTML5 doctype，不区分大小写。
```
<!DOCTYPE html> <!-- 使用 HTML5 doctype，不区分大小写 -->
```
声明文档使用的字符编码
```
<meta charset='utf-8'> <!-- 声明文档使用的字符编码 -->
```
更加标准的 lang 属性写法 http://zhi.hu/XyIa
简体中文
```
<html lang="zh-cmn-Hans"> <!-- 更加标准的 lang 属性写法 http://zhi.hu/XyIa -->
```
繁体中文
```
<html lang="zh-cmn-Hant"> <!-- 更加标准的 lang 属性写法 http://zhi.hu/XyIa -->
```
很少情况才需要加地区代码，通常是为了强调不同地区汉语使用差异，例如：
```
<p lang="zh-cmn-Hans">
<strong lang="zh-cmn-Hans-CN">菠萝</strong>和<strong lang="zh-cmn-Hant-TW">鳳梨</strong>其实是同一种水果。只是大陆和台湾称谓不同，且新加坡、马来西亚一带的称谓也是不同的，称之为<strong lang="zh-cmn-Hans-SG">黄梨</strong>。
</p>
```
优先使用 IE 最新版本和 Chrome
```
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" /> <!-- 优先使用 IE 最新版本和 Chrome -->
```
SEO 优化

页面描述
每个网页都应有一个不超过 150 个字符且能准确反映网页内容的描述标签。文档
```
<meta name="description" content="不超过150个字符" /> <!-- 页面描述 -->
```
页面关键词
```
<meta name="keywords" content=""/> <!-- 页面关键词 -->
```
定义页面标题
```
<title>标题</title>
```
定义网页作者
```
<meta name="author" content="name, email@gmail.com" /> <!-- 网页作者 -->
```
定义网页搜索引擎索引方式，robotterms是一组使用英文逗号「,」分割的值，通常有如下几种取值：none，noindex，nofollow，all，index和follow。文档
```
<meta name="robots" content="index,follow" /> <!-- 搜索引擎抓取 -->
```
可选标签

为移动设备添加 viewport
```
<meta name ="viewport" content ="initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no"> <!-- `width=device-width` 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边 http://bigc.at/ios-webapp-viewport-meta.orz -->
```
width=device-width 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边http://bigc.at/ios-webapp-viewport-meta.orz

content 参数：
```
width viewport 宽度(数值/device-width)
height viewport 高度(数值/device-height)
initial-scale 初始缩放比例
maximum-scale 最大缩放比例
minimum-scale 最小缩放比例
user-scalable 是否允许用户缩放(yes/no)
minimal-ui iOS 7.1 beta 2 中新增属性，可以在页面加载时最小化上下状态栏。这是一个布尔值，可以直接这样写：<meta name="viewport" content="width=device-width, initial-scale=1, minimal-ui">

```
iOS 设备

添加到主屏后的标题（iOS 6 新增）
```
<meta name="apple-mobile-web-app-title" content="标题"> <!-- 添加到主屏后的标题（iOS 6 新增） -->
```
是否启用 WebApp 全屏模式
```
<meta name="apple-mobile-web-app-capable" content="yes" /> <!-- 是否启用 WebApp 全屏模式 -->
```
设置状态栏的背景颜色
```
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" /> <!-- 设置状态栏的背景颜色，只有在 `"apple-mobile-web-app-capable" content="yes"` 时生效 -->
```
只有在 "apple-mobile-web-app-capable" content="yes" 时生效

content 参数：
```
default 默认值。
black 状态栏背景是黑色。
black-translucent 状态栏背景是黑色半透明。
```
如果设置为 default 或 black ,网页内容从状态栏底部开始。

如果设置为 black-translucent ,网页内容充满整个屏幕，顶部会被状态栏遮挡。

禁止数字识自动别为电话号码
```
<meta name="format-detection" content="telephone=no" /> <!-- 禁止数字识自动别为电话号码 -->
```
iOS 图标
rel 参数：

apple-touch-icon 图片自动处理成圆角和高光等效果。

apple-touch-icon-precomposed 禁止系统自动添加效果，直接显示设计原图。

iPhone 和 iTouch，默认 57×57 像素，必须有

```
<link rel="apple-touch-icon-precomposed" href="/apple-touch-icon-57x57-precomposed.png" /> <!-- iPhone 和 iTouch，默认 57x57 像素，必须有 -->
```
iPad，72×72 像素，可以没有，但推荐有
```
<link rel="apple-touch-icon-precomposed" sizes="72x72" href="/apple-touch-icon-72x72-precomposed.png" /> <!-- iPad，72x72 像素，可以没有，但推荐有 -->
```
Retina iPhone 和 Retina iTouch，114×114 像素，可以没有，但推荐有
```
<link rel="apple-touch-icon-precomposed" sizes="114x114" href="/apple-touch-icon-114x114-precomposed.png" /> <!-- Retina iPhone 和 Retina iTouch，114x114 像素，可以没有，但推荐有 -->
```
Retina iPad，144×144 像素，可以没有，但推荐有
```
<link rel="apple-touch-icon-precomposed" sizes="144x144" href="/apple-touch-icon-144x144-precomposed.png" /> <!-- Retina iPad，144x144 像素，可以没有，但推荐有 -->
```
iOS 启动画面
官方文档：https://developer.apple.com/library/ios/qa/qa1686/_index.html

参考文章：http://wxd.ctrip.com/blog/2013/09/ios7-hig-24/

iPad 的启动画面是不包括状态栏区域的。
iPad 竖屏 768 x 1004（标准分辨率）
```
<link rel="apple-touch-startup-image" sizes="768x1004" href="/splash-screen-768x1004.png" /> <!-- iPad 竖屏 768 x 1004（标准分辨率） -->
```
iPad 竖屏 1536×2008（Retina）
```
<link rel="apple-touch-startup-image" sizes="1536x2008" href="/splash-screen-1536x2008.png" /> <!-- iPad 竖屏 1536x2008（Retina） -->
```
iPad 横屏 1024×748（标准分辨率）
```
<link rel="apple-touch-startup-image" sizes="1024x748" href="/Default-Portrait-1024x748.png" /> <!-- iPad 横屏 1024x748（标准分辨率） -->
```
iPad 横屏 2048×1496（Retina）
```
<link rel="apple-touch-startup-image" sizes="2048x1496" href="/splash-screen-2048x1496.png" /> <!-- iPad 横屏 2048x1496（Retina） -->
```
iPhone 和 iPod touch 的启动画面是包含状态栏区域的。
iPhone/iPod Touch 竖屏 320×480 (标准分辨率)
```
<link rel="apple-touch-startup-image" href="/splash-screen-320x480.png" /> <!-- iPhone/iPod Touch 竖屏 320x480 (标准分辨率) -->
```
iPhone/iPod Touch 竖屏 640×960 (Retina)
```
<link rel="apple-touch-startup-image" sizes="640x960" href="/splash-screen-640x960.png" /> <!-- iPhone/iPod Touch 竖屏 640x960 (Retina) -->
```
iPhone 5/iPod Touch 5 竖屏 640×1136 (Retina)
```
<link rel="apple-touch-startup-image" sizes="640x1136" href="/splash-screen-640x1136.png" /> <!-- iPhone 5/iPod Touch 5 竖屏 640x1136 (Retina) -->
```
添加智能 App 广告条 Smart App Banner（iOS 6+ Safari）文档
```
<meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL"> <!-- 添加智能 App 广告条 Smart App Banner（iOS 6+ Safari） -->
```
Windows 8

Windows 8 磁贴颜色
```
<meta name="msapplication-TileColor" content="#000"/> <!-- Windows 8 磁贴颜色 -->
```
Windows 8 磁贴图标
```
<meta name="msapplication-TileImage" content="icon.png"/> <!-- Windows 8 磁贴图标 -->
```
其他

添加 RSS 订阅
```
<link rel="alternate" type="application/rss+xml" title="RSS" href="/rss.xml" /> <!-- 添加 RSS 订阅 -->
```
添加 favicon icon
```
<link rel="shortcut icon" type="image/ico" href="/favicon.ico" /> <!-- 添加 favicon icon -->
```
更多

参阅：

你需要知道的 HTML META
HTML5 Boilerplate explanations and suggestions of header tags




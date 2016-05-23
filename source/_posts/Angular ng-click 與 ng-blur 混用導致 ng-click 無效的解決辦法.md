---
layout: post
title:  Angular ng-click 與 ng-blur 混用導致 ng-click 無效的解決辦法
date: 2015-11-18 23:21:04
tags: angular
---

首先聲明我只是一個 angular 菜鳥，api 都不熟。今天在項目裏遇到一個問題，就是我在實現一個類似 input autocomplete 的功能的時候。當我在點擊下拉列表的時候選中元素之後，想通過 ng-click 事件把選中的元素賦值到輸入框裏。然而在實習這個功能的時候，我還利用通過 ng-blur 的方法隱藏下拉顯示的帶選擇元素。

所以这个时候就会出问题了，因为 ng-blur 事件比 ng-click 先执行的，导致你的 ng-click 事件是无效的。解决办法有几个，我这里记录一个最简单的办法，就是把 ng-click 事件改成 ng-mousedown。其实就和 jquery 里一样的嘛。

下面可以看个简单的列子：

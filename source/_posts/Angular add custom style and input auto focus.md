---
layout: post
title:  Angular add custom style and input auto focus
date: 2015-11-19 11:21:04
tags: JavaScript
---
首先我本人並不是很喜歡 angular 1.X 的，但是項目既然用了，還是要用心去做的。所以從零基礎到直接獨自上手開發肯定會遇到很多坑的，在這裏只是記錄一些遇到的坑，相信很多新人會遇到的。
一：ui.bootstrap 的模態框 modal 組件添加自定義樣式

modal 官方示例的模板是这样的
```
<script type="text/ng-template" id="myModalContent.html">
   <div class="modal-header">
       <h3 class="modal-title">I'm a modal!</h3>
   </div>
   <div class="modal-body">
       <ul>
           <li ng-repeat="item in items">
               <a ng-click="selected.item = item">{{ item }}</a>
           </li>
       </ul>
       Selected: <b>{{ selected.item }}</b>
   </div>
   <div class="modal-footer">
       <button class="btn btn-primary" ng-click="ok()">OK</button>
       <button class="btn btn-warning" ng-click="cancel()">Cancel</button>
   </div>
</script>
```
```
$scope.open = function (size) {

    var modalInstance = $modal.open({
      templateUrl: 'myModalContent.html',
      controller: 'ModalInstanceCtrl',
      size: size,
      resolve: {
        items: function () {
          return $scope.items;
        }
      }
    });

    modalInstance.result.then(function (selectedItem) {
      $scope.selected = selectedItem;
    }, function () {
      $log.info('Modal dismissed at: ' + new Date());
    });
  };
});
```
他只支持 lg,sm 两种大小的弹框，有时候需要定制弹框的大小就有点麻烦了，因为这个 modal 的宽度通过 lg，sm 参数都是在 modal 内容最外层生成的，在 modal-header 的外层，你不可能自己定义 modal-header/body 等宽度样子来设置，当然你不能直接设置 modal-dialog/content 的样式，因为不是这一个地方要用弹窗的，所以我今天看了下，发现是有个解决办法的。那就是，你继续给 open 传参数，但是不是传 lg,sm，传一个你自己定义的一个参数即可，发现这个参数是会在外层 modal 元素标签属性里显示的，也就是说你可以通过属性选择器来设置这个弹框的样式，是不是很巧。比如我传了一个 open('blog')。最终在页面上可以看到最外层弹框里多了一个属性 siz="blog"。那么我们就可以通过 .modal[size="blog"]来单独设置这个弹窗的样式了。

二.自动触发 focus 焦点
angular 实现这个比较简单，写一个通用的指令即可
Demo:

```
<div ng-app="sampleapp">
    <div ng-controller="samplecontoller">

    <div class="focusexample">
        <h1>Auto Focus : </h1>
        With Out Focus : <input type="text"><br><br>
        Auto Focus : <input type="text" focus="true"><br><br>
    </div>

    </div>
</div>
```
```
var myapp = angular.module('sampleapp', [ ]);

myapp.controller('samplecontoller', function ($scope) {


});

angular.module('sampleapp').directive('focus',
	function($timeout) {
		return {
			scope : {
				trigger : '@focus'
			},
			link : function(scope, element) {
				scope.$watch('trigger', function(value) {
					if (value === "true") {
						$timeout(function() {
							element[0].focus();
						});
					}
				});
			}
		};
	}
);
```

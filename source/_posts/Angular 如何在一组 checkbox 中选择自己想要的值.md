---
layout: post
title: Angular 如何在一组 checkbox 中选择自己想要的值
date: 2015-12-20
tags: angular
description: null
---

如何在一组 checkbox 列表了选择自己想要的一个或多个值。我是这么做的：

```html
<label ng-repeat="id in idArr">
  <input
    type="checkbox"
    name="selectedId[]"
    value="{{id}}"
    ng-checked="selection.indexOf(id) > -1"
    ng-click="toggleSelection(id)"
  > {{id}}
</label>
```

```JavaScript
$scope.idArr = ['1', '2', '3', '4'];

// 默认不选中
$scope.selection = [];
$scope.toggleSelection = function toggleSelection(id) {
var idx = $scope.selection.indexOf(id);

if (idx > -1) {
    // 默认选中的
  $scope.selection.splice(idx, 1);
} else {
  $scope.selection.push(if);
}
console.log($scope.selection)
};
```
这样就可以选中一组自己想要的 value 值了



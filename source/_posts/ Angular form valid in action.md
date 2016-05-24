---
layout:     post
title:      Angular form valid in action
date: 2015-12-13
description:
tags: angular
---

Angular 提供了有关表单的属性来帮助我们验证表单. 他们给我们提供了各种有关一个表单及其输入的信息，并且应用到了表单和输入.
>属性类	      描述
$valid	    ng-valid	Boolean 告诉我们这一项当前基于你设定的规则是否验证通过
$invalid	ng-invalid	Boolean 告诉我们这一项当前基于你设定的规则是否验证未通过
$pristine	ng-pristine	Boolean 如果表单或者输入框没有使用则为True
$dirty	    ng-dirty	Boolean 如果表单或者输入框有使用到则为True

Angular 也提供了有关表单及其输入框的类，以便你能够依据每一个状态设置其样式.下面是一个我用的表单验证,表单名为 userForm, 提交表单的方法有多种，我这里采用 ngSubmit 的方法，方法为 saveUserInfo()。

```jade
form(name="userForm",ng-submit="saveUserInfo(userForm.$valid)",novalidate)
    label(for="",ng-class="{ 'has-error' : userForm.telephone.$invalid && !userForm.telephone.$pristine }")
        | 职位
        span(ng-show="userForm.jobTitle.$invalid && !userForm.jobTitle.$pristine") 填写正确的手机号码
        input.form-control(type="text",name="jobTitle",ng-model="user.jobTitle",required)
    label(for="",ng-class="{ 'has-error' : userForm.nick.$invalid && !userForm.nick.$pristine }")
        | 用户昵称
        span(ng-show="userForm.nick.$invalid && !userForm.nick.$pristine") 填写正确的手机号码
        input.form-control(type="text",name="nick",ng-model="user.nick",required)
    label(for="",ng-class="{ 'has-error' : userForm.email.$invalid && !userForm.telephone.$pristine }")
        | 邮箱
        span(ng-show="userForm.email.$invalid && !userForm.email.$pristine") 填写正确的手机号码
        input.form-control(type="text",name="email",ng-model="user.email",required)
    label(for="",ng-class="{ 'has-error' : userForm.telephone.$invalid && !userForm.telephone.$pristine }")
        | 联系电话
        span(ng-show="userForm.telephone.$invalid && !userForm.telephone.$pristine") 填写正确的手机号码
        input.form-control(type="text",name="telephone",ng-model="user.telephone",ng-pattern="/1[3|5|7|8|][0-9]{9}/",required)
    button.btn.btn-primary(type="submit",ng-disabled="userForm.$invalid") 确认
    button.btn.btn-default(ng-click="cancel()") 取消
```
有几点可以分析一下，首先一定要每个 input 要有 name 属性，给每个 input 输入框准备一个错误提醒的消息，就是上面的 span 标签。这里我们只想使用 angular 自带的验证，需要关闭浏览器对 html5 表单的原生验证，所以给表单加上 novalidate 属性.

如果需要每个表单都必填，那么给 input 添加 required 属性，顺便给提交按钮添加  disabled 属性，当有一个表单的校验没通过，这个按钮都是灰色不可用的。

还有表单一个常用的 radio 类型没有提到，这里也顺便一起说了，有时候我们需要做一些单项选择，而且可以二次修改，所以如果之前这个 radio 已经选好了某个选项，那么下次修改他的时候，它默认也应该是选中的状态。这个利用双向绑定很容易实现的:
```jade
label(for="")
    | 性别：
    label.radio-inline.c-radio
        input(type='radio',ng-trim="",name="sex",ng-click="user.sex=1",value="1",ng-model="user.sex")
        span.iconfont.icon-check
        |  男
    label.radio-inline.c-radio
        input(type='radio',ng-trim="",name="sex",ng-click="user.sex=2",value="2",ng-model="user.sex")
        span.iconfont.icon-check
        |  女
```
只要给对应的选项添加对应选中的 value 值，绑定 ngModel 即可。
上面这些都是一些基本的 angualr 表单的使用。没啥技巧，都是一些死东西，第一次明白后面基本照用就是。



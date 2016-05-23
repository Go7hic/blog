---
layout: post
title:  Angular 在低版本 chrome 内核浏览器无法记住密码的坑
date: 2015-10-25 11:21:04
tags: angular
---
又是一个坑，今天有人反应我们刚上的项目在 360浏览器极速模式下面记住密码无效，看起来虽然浏览器记住了，里面也有值，但是提交登录的时候提示没有账户名和密码。可是在我的最新版 chrome 浏览器里是可以记住的，也是可以登录的。咋就在你这国产 chrome 里面就没效呢，在 360 浏览器下调试了一下，发现提价的时候果然账户密码的数据没有绑定上。WTF,这是啥坑，是 angular 还是 chrome  的呢，遇 bug 别急，先 google 和 栈爆找一下。

找了一下，很少有这个问题的，但还是被窝找到了，看这 https://github.com/angular/angular.js/issues/1460 或者 https://github.com/fnakstad/angular-client-side-auth/issues/58#issuecomment-147861575 。可以看到好像是旧版 chrome 的和 angular 的一个 bug。就是说你不用 angular 在低版本 chrome 是没有这个问题的，但是你在低版本的 chrome 里用 angular 是有这个问题的。好了，问题找到了，接下来就是修复了，我看到网上有修复这个 bug 的一个 angular 指令。但是在我调试的时候，发现这个登录框实际上是有记住的账户和密码值的，只是数据绑定没有给帮上，所以，这个地方，我们可以不用数据绑定呀，直接用 `angular.elemnt('.account-input').val()` 这样就可以获取里面的值呀，多方便是吧。

一开始我们是这样写的

```jade
form.form-validate(role="form", ng-submit="login()", name="loginForm", novalidate="").mb-lg
    .form-group.has-feedback
        input.form-control.login-name(type="text", ng-trim="" name="account_username" placeholder="请输入用户名", autocomplete="off", ng-model="account.username", required="")
        span.fa.fa-user.form-control-feedback.text-muted
        span.text-danger(ng-show="loginForm.account_username.$dirty && loginForm.account_username.$error.required") 请输入用户名
        span.text-danger(ng-show="loginForm.account_username.$dirty && loginForm.account_username.$error.email") 用户名无效
    .form-group.has-feedback
        input.form-control.login-password(type="password", name="account_password" placeholder="请输入密码", ng-model="account.password", required="")
        span.fa.fa-lock.form-control-feedback.text-muted
        span.text-danger(ng-show="loginForm.account_password.$dirty && loginForm.account_password.$error.required") 请输入密码
```
```
$scope.account = {
    username: '',
    password: ''
};
$scope.login = function () {

    if ($scope.account.password !== '' && $scope.account.username !== '' ) {
        AuthService.login({
            username: $scope.account.username,
            password: $scope.account.password
        }, function () {
            // 登录成功后需要进一步获取管理员信息
            AdminService.get(
                {},
                function () {
                }, function () {
                }
            );
            $rootScope.$broadcast(AUTH_EVENTS.loginSuccess);
        }, function (error) {
            $scope.authMsg = error.message;
            $rootScope.$broadcast(AUTH_EVENTS.loginFailed, error.code);
        });
    } else {
        $scope.loginForm.account_username.$dirty = true;
        $scope.loginForm.account_password.$dirty = true;
    }
};
```
上面的代码在新的 chrome 历史没问题的，可以获取绑定的帐号密码，但是低版本有问题，我们可以直接通过获取 dom 的值来获取。修改后这样：
```
$scope.login = function () {
$scope.account = {
    username: angular.element('.login-name').val(),
    password: angular.element('.login-password').val()
};
    if ($scope.account.password !== '' && $scope.account.username !== '' ) {
        AuthService.login({
            username: $scope.account.username,
            password: $scope.account.password
        }, function () {
            // 登录成功后需要进一步获取管理员信息
            AdminService.get(
                {},
                function () {
                }, function () {
                }
            );
            $rootScope.$broadcast(AUTH_EVENTS.loginSuccess);
        }, function (error) {
            $scope.authMsg = error.message;
            $rootScope.$broadcast(AUTH_EVENTS.loginFailed, error.code);
        });
    } else {
        $scope.loginForm.account_username.$dirty = true;
        $scope.loginForm.account_password.$dirty = true;
    }
};
```
当然还有其他的方法，比如新建一个隐藏表单，还有封装一个指令，但那些太麻烦了，增加一些没意义的代码。

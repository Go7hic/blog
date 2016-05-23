---
layout:     post
title:     修改 gulp-strip-debug 配置，不要删除 alert 调试方法
date: 2015-08-24
tags: gulp
description: 
---


事情起因是这样的，公司一项目在 ie9 下面有点问题，但是打开控制台调试的时候又好了。调查发现原来 ie9 不支持代码里写一些 console 对象的方法，只有打开控制台才能触发。而我们平时开发的时候喜欢写一些 console 这样的调试代码，我们以为用 Grunt 压缩后会去掉这些调试代码，但是同事发现只会去除 console.log 这个方法，而其他的 console.error 这些方法不能去掉。
<!-- more -->
遇到这个情况后，我看了下自己的项目，发现压缩后 console 的调式代码也没有清除干净，于是又找了一个插件，因为我用的是 gulp 构建，所以找到了 gulp-strip-debug 这个 gulp 插件。压缩后查了一下，嗯，果然把 console 的代码全清理了，果然很吊。但是后来我又发现一个问题，这个插件有三个模块console,alert,debuger，也就是说这个插件还会把代码里的 alert 方法也删掉。这个我可不想要，因为有时候我还是想弹个框给提示啥的，虽然不主张直接弹框给用户，但是也有例外嘛。所以我想看下怎么才可以在使用插件的时候配置一下不要 删除 alert 这个功能。配置了一下还是有效的，简单修改如下：

使用的时候这样写

```
stripDebug({debugger: true, console: true, alert: false});
```

然后打开  gulp-strip-debug/index.js 换成下面的，其实就是加了个 options 参数

```
'use strict';
var gutil = require('gulp-util');
var through = require('through2');
var stripDebug = require('strip-debug');

module.exports = function (options) {
    return through.obj(function (file, enc, cb) {
        if (file.isNull()) {
            cb(null, file);
            return;
        }

        if (file.isStream()) {
            cb(new gutil.PluginError('gulp-strip-debug', 'Streaming not supported'));
            return;
        }

        try {
            file.contents = new Buffer(stripDebug(file.contents.toString(), options).toString());
            this.push(file);
        } catch (err) {
            this.emit('error', new gutil.PluginError('gulp-strip-debug', err, {fileName: file.path}));
        }

        cb();
    });
};

```

然后打开 gulp-strip-debug/node_modules/strip-debug/index.js ，换成下面的，也是加了个参数，然后判断参数进行运算

```
'use strict';
var rocambole = require('rocambole');
var stripDebugger = require('rocambole-strip-debugger');
var stripConsole = require('rocambole-strip-console');
var stripAlert = require('rocambole-strip-alert');

// esprima@2.1 introduces a "handler" property on TryStatement, so we would
// loop the same node twice (see jquery/esprima/issues/1031 and #264)
rocambole.BYPASS_RECURSION.handler = true;

module.exports = function (src, options) {
    options = options || {};

    return rocambole.moonwalk(src, function (node) {
        options.debugger !== false && stripDebugger(node);
        options.console !== false && stripConsole(node);
        options.alert !== false && stripAlert(node);
    });
};

```
 修改完之后就可以放心用了，你可以自由的配置你想删除哪个 调式代码的方法。
 


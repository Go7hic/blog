---
layout: post
title:  JavaScript客户端检测
date: 2014-03-24 11:11:04
tags: JavaScript
---
web客户端检测手段很多，而且各有利弊，不到万不得已不推荐使用客户端检测。客户端检测也有几种，这里主要讲用户代理字符串检测。

一：能力检测（特性检测）

能力检测不是识别特定的浏览器，而是识别浏览器的能力。

二：怪癖检测

和能力检测不同，它主要是识别浏览器的bug，而且各个浏览气的bug应该都是独有的。

三：用户代理检测

用户代理检测通过检测用户代理字符串来确定实际使用的浏览器。这里我们可以先了解下用户代理字符串的历史http://www.cnblogs.com/wangtuda/p/3405495.html 。

用户代理字符串检测技术主要检测以下几个信息:

识别呈现引擎
识别浏览器
识别平台
识别windows操作系统
识别移动设备
识别游戏系统
以下是完整的用户代理字符串检测脚本
```
var client = function() {
    //呈现引擎
    var engine = {
        ie:0,
        gecko:0,
        webkit:0,
        khtml:0,
        opera:0,
        //完整的版本号
        ver:null
    };
    //浏览器
    var browser = {
        //主要浏览器
        ie:0,
        firefox:0,
        konq:0,
        opera:0,
        chrome:0,
        safari:0,
        //具体版本号
        ver:null
    };
    //平台、设备及操作系统
    var system = {
        win:false,
        mac:false,
        x11:false,
        //移动设备
        iphone:false,
        ipod:false,
        nokiaN:false,
        winMobile:false,
        macMobile:false,
        //游戏系统
        wii:false,
        ps:false
    };
    //检测呈现引擎和浏览器
    var ua = navigator.userAgent;
    if (window.opera){
        engine.ver = browser.ver = window.opera.version();
        engine.opera = browser.opera = parseFloat(engine.ver);
    }
    else if(/AppleWebKit\/(\S+)/.test(ua)){
        engine.ver = RegExp[“$1”];
        engine.webkit = parseFloat(engine.ver);
        //确定是Chrome还是Safari
        if(/Chrome\/(\S+)/.test(ua)){
            browser.ver = RegExp[“$1”];
            browser.chrome = parseFloat(browser.ver);
        }
        else if(/Version\/(\S+)/.test(ua)){
            browser.ver = RegExp[“$1”];
            browser.safari = parseFloat(browser.ver);
        }
        else{
            //近似确定版本号
            var safariVersion = 1;
            if(engine.webkit < 100){
                safariVersion = 1;
            }
            else if(engine.webkit < 312){
                safariVersion = 1.2;
            }
            else if(engine.webkit < 412){
                safariVersion = 1.3;
            }
            else{
                safariVersion = 2;
            }
            browser.safari = browser.ver = safariVersion;
        }
    }
    else if(/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)){
        engine.ver = browser.ver = RegExp[“$1”];
        engine.khtml = browser.konq = parseFloat(engine.ver);
    }
    else if(/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)){
        engine.ver = RegExp[“$1”];
        engine.gecko = parseFloat(engine.ver);
        //确定是不是Firefox
        if(/Firefox\/(S+)/.test(ua)){
            browser.ver = RegExp[“$1”];
            browser.firefox = parseFloat(browser.ver);
        }
    }
    else if(/MSIE ([^;]+)/.test(ua)){
        engine.ver = browser.ver = RegExp[“$1”];
        engine.ie = browser.ie = parseFloat(engine.ver);
    }
    //检测浏览器
    browser.ie = engine.ie;
    browser.opera = engine.opera;
    //检测平台
    var p = navigator.platform;
    system.win = p.indexOf(“Win”) == 0;
    system.mac = p.indexOf(“Mac”) == 0;
    system.x11 = (p == “X11”) || (p.indexOf(“Linux”) == 0);
    //检测操作系统
    if(system.win){
        if(/Win(?:dows)?([^do]{2})\s?(\d+\. \d+)?/.test(ua)){
            if(RegExp[“$1”] == “NT”){
                switch(RegExp[“$2”]){
                    case “5.0”:
                        system.win = “2000”;
                        break;
                    case “5.1”:
                        system.win = “XP”;
                        break;
                    case “6.0”:
                        system.win =”Vista”;
                        break;
                    default:
                        system.win = “NT”;
                        break;
                }
            }
            else if(RegExp[“$1”] == “9x”){
                system.win = “ME”;
            }
            else{
                system.win = RegExp[“$1”];
            }
        }
    }
    //移动设备
    system.iphone = ua.indexOf(“iPhone”) > -1;
    system.iphod = ua.indexOf(“iPod”) > -1;
    system.nokiaN = ua.indexOf(“NokiaN”) > -1;
    system.winMobile = (system.win = “CE”);
    system.macMobile = (system.iPhone || system.iPod);
    //游戏系统
    system.wii = ua.indexOf(“Wii”) > 1;
    system.ps = /playstation/i.test(ua);
    //返回这些对象
    return {
        engine: engine,
        browser: browser,
        system: system
    };
}();
```
最后我们在使用客户端检测方法时，优先考虑使用能力检测，最后才考虑用户代理检测。


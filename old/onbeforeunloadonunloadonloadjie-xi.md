---
title: onbeforeunloadonunloadonloadjie-xi
date: 2017-04-18 17:03:08
tags: jquery
categories: web
---

### onbeforeunload事件
- 语法：

```
-- html中
<element onbeforeunload="myScript">

-- javaScript 中
object.onbeforeunload=function(){myScript};

-- javaScript 中使用addEventListener()方法：
object.addEventListener("beforeunload", myScript);
```

- 浏览器支持
    - chrome、ie、火狐、等都支持，360极速浏览器的兼容模式并不支持
- 定义及用法：
    - onbeforeunload 事件在即将离开当前页面（刷新或关闭）时触发。
    - 该事件可用于弹出对话框，提示用户是继续浏览页面还是离开当前页面。
    ```
    window.onbeforeunload = function (e) {
    // js代码
    e = e || window.event;
    // For IE and Firefox prior to version 4
    if (e) {
    e.returnValue = i18n("v4.js.quitCourse");
    }
    // For Safari
    return i18n("v4.js.quitCourse");
    };
    ```
- 注意：该方法并不可靠，有一定几率关闭浏览器的时候并不会执行里面的js代码，如果加上return 提示信息给一个缓冲的时间，有可能在低版本的ie浏览器中会出现a标签也会触发该事件。

### onunload事件
- 语法：
```
-- html中
<body onunload="SomeJavaScriptCode">
-- javaScript中
window.onunload=function(){SomeJavaScriptCode};
```
- 定义和语法
    + onunload 事件在用户退出页面时发生。
    + onunload 发生于当用户离开页面时发生的事件(通过点击一个连接，提交表单，关闭浏览器窗口等等。)
    + 注意：onunload 事件同样触发了页面载入事件(+ onload 事件)。
### onload事件
- 语法
```
<body onload="SomeJavaScriptCode">
window.onload=function(){SomeJavaScriptCode};
```
- 定义和语法
    + onload 事件会在页面或图像加载完成后立即发生
    + onload 通常用于 <body> 元素，在页面完全载入后(包括图片、css文件等等。)执行脚本代码。



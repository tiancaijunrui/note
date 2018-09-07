---
title: web前端知识点
date: 2017-02-17 12:07:35
tags: jquery
categories: web
---

### 1.业务需求：是浏览器焦点聚焦到新打开的页面
- 常用方法：window.focus()
```
<html>
<body>
<script type="text/javascript">
myWindow = window.open('url','tab_name','width=200,height=100');
myWindow.document.write("This is 'myWindow'");
myWindow.focus();
</script>
</body>
</html>
```

- 上面的方法无效的办法,或者已经打开了一个窗口，需要打开一个新窗口覆盖原来的窗口并聚焦到新的页面
- 首先正常打开一个窗口
```
<html>
<body>
<script type="text/javascript">
window.myWindow= window.open('url','tab_name');
window.myWindow.focus();
</script>
</body>
</html>
```
- 然后需要打开另一个新窗口覆盖旧窗口
```
if(window.myWindow){
window.myWindow.close();
}
window.myWindow= window.open(_url, '_courseInfo');
$(window.myWindow).focus();
```
### 2.前台传值是url等特殊字符的话
```
$.ajax({
url:"",
escapeXml:true,
data:"",
success:function(data){
}
})
```
### 3.JS代码浏览器兼容性-newDate()
- 无参
```
// 所有浏览器都兼容
var dateTime = new Date();
```
- 日期参数（格式一）
```
var dateTime = new Date("2016-05-20");
// ie9 以下的不兼容 ie9 及以上的兼容
// 火狐、google浏览器兼容
```
- 日期参数（格式二）
```
var dateTime = new Date("2016/05/20");
// 所有浏览器都兼容
```
- 日期时间参数（格式一）
```
// ie 所有版本都不兼容
// 火狐不兼容、google兼容
var dateTime = new Date("2016-05-20 23:41:00");
```
- 日期时间参数 （格式二）
```
// 所有浏览器都兼容
var dateTime = new Date("2016/05/20 23:42:00");
```
- 日期时间参数（格式三）
```
// ie9 以下不兼容，ie9以上兼容
// ie9 半兼容（可以得到日期时间，但是时间是错误的,加了8个小时。比如dateTime的时间将会是2016-05-21 07:42:00）
// 火狐兼容、google半兼容同ie9 時間多加8小時
var dateTime = new Date("2016-05-20T23:42:00");
```
- 日期时间参数 （格式四）
```
// ie 半兼容，所有版本时间都多加了一个小时，即得到的dateTime的值为：2016-05-21 00：42：00
// 火狐不兼容，google浏览器不兼容
var dateTime = new Date("2016/05/20T23:42:00")
```
### 4.ie版本切换解析页面

```
<#--<meta http-equiv="X-UA-Compatible" content="IE=9;IE=8;IE=7;"/>-->
<meta http-equiv="X-UA-Compatible" content="IE=edge"/>

```

### 5.使用jquery获取url及url参数的方法
```
//获取url中的参数
function getUrlParam(name) {
var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)"); //构造一个含有目标参数的正则表达式对象
var r = window.location.search.substr(1).match(reg); //匹配目标参数
if (r != null) return unescape(r[2]); return null; //返回参数值
}
使用方法：
var xx = getUrlParam('reurl');
```

```
// 可以通过这个方法为jquery扩展一个方法来通过jquery获取url参数
(function ($) {
$.getUrlParam = function (name) {
var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
var r = window.location.search.substr(1).match(reg);
if (r != null) return unescape(r[2]); return null;
}
})(jQuery);
使用方法：
var xx = $.getUrlParam('reurl');
```

### 6.javaScript 页面跳转的几种方式
- 第一种
```
window.location.href="url";
```
- 第二种
```
alert("返回");
window.history.back(-1);
```
- 第三种
```
window.navigate("url");
```
- 第四种
```
self.location="url";
```
- 第五种
```
alert("非法访问！")；
top.location="url";
```
### 7.解决前台传值是url等特殊字符的处理方式
```
$.ajax({
url:"",
type:"POST",
escapeXml:true,
data:"",
...
})
```


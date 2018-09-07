---
title: 2017nian11yue6ri
tags: java
categories: java
---

### 1.jquery金钱校验 正则表达式

```
var reg = /(^[1-9]([0-9]+)?(\.[0-9]{1,2})?$)|(^(0){1}$)|(^[0-9]\.[0-9]([0-9])?$)/;
var money = "546.23"
//000 错
//0 对
//0. 错
//0.0 对
//050 错
//00050.12错
//70.1 对
//70.11 对
//70.111错
//500 正确
if(reg.test(money)){
	alert("正确~");
}else{
	alert("错误~");
}

```

### 2.BigDecimal格式化小数点

```
setScale(1) 表示保留一位小数，默认用四舍五入方式
setScale(1,BigDecimal.ROUND_DOWN)直接删除多余的小数位
setScale(1,BigDecimal.ROUND_UP)进位处理
setScale(1,BigDecimal.ROUND_HALF_UP)四舍五入 如果最后一位是5 则向前进一位
setScale(1,BigDecimal.ROUND_HALF_DOWN) 如果最后一位是5，则向下舍
```
    
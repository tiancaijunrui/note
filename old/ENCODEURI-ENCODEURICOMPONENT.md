---
title: encodeURI-encodeURIComponent
date: 2018-04-23 16:49:06
tags: web,jquery
---

### encodeURI、encodeURIComponent的区别

- encodeURI 不会进行编码的字符有82个：

```
!，#，$，&，'，(，)，*，+，,，-，.，/，:，;，=，?，@，_，~，0-9，a-z，A-Z
```

- encodeURIComponent 不会进行编码的字符有71个：

```
!， '，(，)，*，-，.，_，~，0-9，a-z，A-Z
```

- encodeURI 主要用于直接赋值给地址栏的时候：

```
 location.href=encodeURI("http://huangjacky.com/");
```

- encodeURIComponent 主要用于url的query参数

```
 location.href="http://huangjacky.com/test.php?a="+encodeURIComponent("param");
```

`参考链接：https://www.jianshu.com/p/075f5567c9a1`


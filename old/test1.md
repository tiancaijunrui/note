---
title: 基于postgresql的一些技巧
date: 2017-02-16 17:24:46
tags: postgresql
categories: sql
---
### 1.业务需求：查询一个字段想要中间用逗号隔开在一列中展示：

```
select string_agg(course_code,',') from t_els_course_info where corp_code = 'wf007' and create_corp_code = 'wf007' 
and course_standard <> 'OLINEURL' LIMIT 500;
```

### 2.业务需求：查询的数据需要在其他sql中直接在in里用，就是字段两边有单引号，后面有一个逗号的样式：

```
select ''''||course_code||''',' from t_els_course_info where corp_code = 'wf007'
and create_corp_code = 'wf007' and course_standard <> 'OLINEURL' LIMIT 500;
```
### 3.sql执行顺序

![](/images/sql.png)
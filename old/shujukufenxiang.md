---
title: postgresql数据库
date: 2017-04-11 18:51:43
tags: postgresql
categories: sql
---

### 1.postgresql 简介
- 介绍
	- 开源，免费，功能强大的关系型数据库（SQLite,MySQL）
- 连接信息
	```
	<bean name="myDatasource" class="org.apache.commons.dbcp.BasicDataSource">
	<property name="driverClassName" value="org.postgresql.Driver"/>
	<property name="username" value="postgres"/>
	<property name="password" value="admin"/>
	<property name="url" value="jdbc:postgresql://127.0.0.1:5432/game"/>
	</bean>
	```

### 2.sql语句（增删改查）
- 操作表结构
- 添加数据库
	```
	create DATABASE demo;
	```
- 删除数据库
	```
	DROP DATABASE demo;
	```
- 建表
	```
	CREATE TABLE demo (
	id VARCHAR(32) NOT NULL,
	name VARCHAR(200) NOT NULL,
	PRIMARY KEY(id)
	);
	COMMENT ON COLUMN "demo"."id" IS '主键';
	COMMENT ON COLUMN "demo"."name" is '用户名';
	```
	- 删除表
	```
	DROP TABLE demo;
	```
	- 新增字段
	```
	ALTER TABLE demo ADD age int4 ;
	COMMENT ON COLUMN "demo"."age" IS '年龄';
	```
	- 删除字段
	```
	ALTER TABLE demo DROP COLUMN age;
	```
	- 修改字段
	```
	ALTER TABLE demo ALTER COLUMN age TYPE float8;
	```
- 数据的简单查询
	- 增
	```
	INSERT INTO demo VALUES('da231d7197864dfd825d7a975c35755b','demo1',null);
	INSERT INTO demo VALUES('601d7fcd4a014e6aa304f49032180ba3','demo2',12);
	```
	- 删
	```
	DELETE FROM demo where name = 'demo1';
	```
	- 改
	```
	UPDATE demo set name = 'demo3',age = 22 WHERE name = 'demo1';
	```
	- 查
	```
	SELECT * from demo where name LIKE '%demo%' LIMIT 2 ;
	```
- 常用关键字和语法
	- like
	```
	-- 包含字符mo3
	select * from demo where name like '%mo3%';
	-- 第一个字符
	select * from demo where name like '_emo3';
	select * from demo where name like 'd_m_3';
	```
	- in
	```
	select * from demo where id in (
	'da231d7197864dfd825d7a975c35755b',
	'601d7fcd4a014e6aa304f49032180ba3'
	);
	--mdl中in 不能使用占位符？
	builder.append(" and t.teacherCategoryId in (:teacherCategoryIds)");
	builder.addParameter("teacherCategoryIds", teacherCategoryList);
	```
	- between
	```
	--操作符 BETWEEN ... AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。
	select create_time from t_els_course_info where corp_code = 'ladeng.com' and create_time BETWEEN '2017-4-11' and '2017-4-12' LIMIT 10;
	-- 包括 and 不包括
	select create_time from t_els_course_info where corp_code = 'ladeng.com' and create_time >= '2017-4-11' and create_time < '2017-4-12' LIMIT 10;
	```
	- left join
	```
	select * from t_els_pro_user pu LEFT JOIN t_els_pro_course pc on pc.pro_id = pu.pro_id where pu.user_id = '' and pc.course_id = '' and pu.corp_code = '';
	```
	- count()
	```
	select "count"(*) from t_els_course_info where corp_code = 'ladeng.com';
	--mdl中分页pata.total 使用的是 com.tbc.paas.mdl.impl.MdlDataServiceImpl#executeRowTypeUniqueQuery 这个方法用Long接收
	```
	- GROUP BY,SUM(),MAX(),MIN(),AVG()
	```
	--GROUP BY 语句用于结合合计函数，根据一个或多个列对结果集进行分组,sum,max,min,avg null 值不包括在计算中。
	select rs.stage_id,"count"(rscr.course_id),"max"(rs.must_total_score),"max"(rs.elective_total_score),
	"max"(rs.must_total_period),"max"(rs.elective_total_period)
	from t_els_rm_stage rs INNER JOIN t_els_rm_stage_course_rel rscr on rs.stage_id = rscr.stage_id
	and rscr.corp_code = 'ladeng.com'
	where rs.stage_id in (
	select stage_id from t_els_rm_stage where road_map_id = 'fdfd5eb8263442349e83d53191e13431' and corp_code = 'ladeng.com'
	) AND rs.corp_code = 'ladeng.com' GROUP BY rs.stage_id
	```
	- LEN（LENGTH）,NOW()
	```
	select * from t_els_course_info where corp_code = 'ladeng.com' and LENGTH(cover_id) > 20 AND create_time < now() LIMIT 10;
	```
	- FORMAT(to_char)
	```
	select to_char(create_time , 'YYYY-MM-dd') from t_els_course_info where corp_code = 'ladeng.com' and create_time is not null LIMIT 10;
	```
	- 业务场景
	```
	--查询一个字段想要中间用逗号隔开在一列中展示
	select string_agg(course_code,',') from t_els_course_info where corp_code = 'wf007' and create_corp_code = 'wf007'
	and course_standard <> 'OLINEURL' LIMIT 500;
	--查询的数据需要在其他sql中直接在in里用，就是字段两边有单引号，后面有一个逗号的样式：
	select ''''||course_code||''',' from t_els_course_info where corp_code = 'wf007'
	and create_corp_code = 'wf007' and course_standard <> 'OLINEURL' LIMIT 500;
	```


### 3.执行顺序
![](/images/sql.png)

### 4.索引
- 什么是索引

	- 索引是对数据库中一列或几列的数据按照特定的数据结构进行排序保存的一种方式。使用索引可以加快数据库查询或排序时的速度。如果不使用索引那么查询数据时就会进行全表扫描也就是每条数据读取一遍看是不是要找的值，而使用索引可以快速找到要找的数据，不用扫描所有数据。
- 索引的优点，缺点

	- 可以大大提高数据查询的速度
	在实现数据的参考完整性方面，可以加快表和表之间的连接
	在分组和排序时，可以减少分组和排序的时间
	- 创建和维护索引需要耗费时间，数据量越大耗费时间也越长
	索引需要占用额外的磁盘空间，如果数据量很大又有大量索引，那么索引文件大小增加很快
	对表中数据改动的时候，索引需要动态维护，降低了数据操作的速度
- 类型

	- PostgreSQL提供的索引类型有： B-tree、Hash、GiST和GIN。大多数情况下，我们使用比较常用的B-tree索 引。
	1.B-Tree索引主要用于等于和范围查询，特别是当索引列包含操作符" <、<=、=、>=和>"作为查询条件时，PostgreSQL的查询规划器都会考虑使用B-Tree索引。在使用BETWEEN、IN、IS NULL和IS NOT NULL的查询中，PostgreSQL也可以使用B-Tree索引。然而对于基于模式匹配操作符的查询，如LIKE、ILIKE、~和 ~*，仅当模式存在一个常量，且该常量位于模式字符串的开头时，如col LIKE 'foo%'或col ~ '^foo'，索引才会生效，否则将会执行全表扫描，如：col LIKE '%bar'。<br>
	2.Hash索引只能处理简单的等于比较。当一个字段涉及到使用“=”操作符进行比较时查询规划器会考虑使用Hash索 引。<br>
	3.GiST索引不只是一种索引，还是一种架构，可以实现很多不同的索引策略。所以，GiST索引可以使用的特定操作 符类型高度依赖于索引策略(操作符类)。<br>
	4.GIN索引是反转索引，它可以处理包含多个键的值(比如数组)。和GiST索引类似，GIN支持用户定义的索引策略， GIN索引可以使用的特定操作符类型根据索引策略不同而不同。
### 5.参考资料
- [SQL 教程](http://www.w3school.com.cn/sql/)
- [PostgreSQL](http://www.jianshu.com/p/161b66eca691)
- [PostgreSQL学习手册(索引)](http://www.cnblogs.com/stephen-liu74/archive/2011/12/22/2298182.html)
- [我的博客](http://junrui.online/)



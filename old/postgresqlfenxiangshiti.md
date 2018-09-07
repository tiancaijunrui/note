---
title: postgresql-分享试题及答案
date: 2017-04-21 15:46:53
tags: postgresql
categories: sql 
---

### 1.请写出配置数据库连接信息，必须要配置的属性<5>：
### 2.请写出配置数据库连接信息中url属性的书写格式<5>：
### 3.建一张user表，其中字段包含主键id（字符串），姓名（字符串），性别（布尔值）,并添加注释，给姓名添加索引，请写出建表语句<10>：
### 4.对user表添加字段年龄（整型），并添加注释，然后修改该字段为浮点型，请写出对应sql<5>：
### 5.person 表 的数据如下所示
```
Id  LastName    FirstName       Address        City
1   Adams           John        Oxford Street   London
2   Bush            George      Fifth Avenue    New York
3   Carter          Thomas      Changan Street  Beijing
4   tiancai         junrui       Oxford Street  London

```
- 5.1 从 "Persons" 表中选取居住城市包含 "jing" 或者address 包含 'street' 的人<5>：
- 5.2 从 "Persons" 表中选取居住城市为 London 并且 LastName 包含 'Adam' 的人的数量（要求通配符与5.1 不一致）<5>：
### 6. courseInfo表的数据如下所示
```
course_id                           create_time
bc687adbecb440ac8fa28d54f904953a    2017-04-16 10:16:44.395
3b57b691d6f643df961cb29a15d3f306    2017-04-15 10:24:00.662
c34d8f5a9baf4dad98386c524ec55533    2017-04-14 10:26:57.784
576b9f3354c84f3f83ab9e2fec10c8db    2017-04-13 10:31:58.994
00b4a76405784bbba63cf6b6fe1e0e76    2017-04-12 10:38:11.204
6617d0799fb749f7aa1f8d724adcdcac    2017-04-11 10:43:53.803
```
- 6.1 请用between。。。and 筛选出创建时间在 2017-4-11 和2017-10-1的课程id<5>
- 6.2 请不使用between。。。and 实现6.1的需求<5>
- 6.3 请写出between A and B ，A B 可以是哪些类型<5>
### 7. person、Orders表数据如下所示
```
-- person
Id_P    LastName    FirstName   Address City
1   Adams   John    Oxford Street   London
2   Bush    George  Fifth Avenue    New York
3   Carter  Thomas  Changan Street  Beijing
-- Orders
Id_O    OrderNo Id_P
1   77895   3
2   null    3
3   22456   1
4   24562   1
5   34764   65
```
- 7.1 请写出以下sql的执行结果<5>
    ```
    SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo FROM Persons, Orders WHERE Persons.Id_P = Orders.Id_P ;
    ```
- 7.2 请写出以下sql的执行结果<5>
     ```
     SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo FROM Persons LEFT JOIN Orders ON Persons.Id_P=Orders.Id_P ORDER BY Persons.LastName
     ```
- 7.3 请写出以下sql的执行结果<5>
    ```
    SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo FROM Persons RIGHT JOIN Orders ON Persons.Id_P=Orders.Id_P ORDER BY Persons.LastName
    ```
### 8.Orders表如下所示
```
O_Id    OrderDate   OrderPrice  Customer
1   2008/12/29  1000    Bush
2   2008/11/23  1600    Carter
3   2008/10/05  700 Bush
4   2008/09/28  300 Bush
5   2008/08/06  2000    Adams
6   2008/07/21  100 Carter
```
- 8.1 如果想要达到如下结果，请写出sql：<5>
    ```
    Customer    SUM(OrderPrice)
    Bush        2000
    Carter      1700
    Adams       2000
    ```
### 9.courseInfo 表数据如下
    ```
    id    cover_id                    create_time             online
    1    kuaike_cover_red_0100  2014-04-10 19:33:06.862     t
    2   56c0334d45ce278d537b3f08_0100   2016-02-14 15:56:45.534 null
    3    kuaike_cover_green_0100        2014-04-10 19:33:07.136     f
    4    56c5116845ce278d537b434b_0100  2016-02-18 08:33:33.127    t
    5   54cdb4e145cee74285e8254b_0100   2015-02-01 13:06:48.301     null
    ```
- 9.1请写出cover_id长度大于20 创建时间在207-2-23 至今的 课程信息 的sql（数据库：postgresql），其中创建时间以‘2017年5月2日’的形式呈现：<5>
- 9.2请写出执行下列sql后的结果<5>
```
select id from courseInfo where online is not true;
```
### 10.简单说一说drop、delete与truncate的区别<5>
### 11.第九题的数据表，请写出在9.1中的需求基础上，按照创建时间排序（最新的数据在上面），第十一条数据到第二十条数据的sql：<5>
### 12.索引的优点，缺点,PostgreSQL提供的索引类型:<5>
### 13.有testA表数据如下
```
id  ta_ysear    ta_num
1   2001    500
2   2002    300
3   2003    600
```
- 13.1有如下业务需求，请写出sql：<5>
```
year    sum
2003    1400
2002    800
2001    500
```
### 14.第13题的数据表，请写出满足如下业务需求的sql:<5>
```
表名：商品表
名称   产地             进价
苹果   烟台                2.5
苹果   云南                1.9
苹果   四川                3
西瓜   江西                1.5
西瓜   北京                2.4
给出平均进价在2元以下的商品名称
```






## 参考答案
### 1.driver,username,password,url
### 2.参考答案
```
jdbc:数据库类型://host:port/database[?参数=][参数值&参数=][参数值]
```
### 3.参考答案
```
CREATE TABLE demo (
id VARCHAR(32) NOT NULL,
name VARCHAR(200) NOT NULL,
sex BOOLEAN DEFAULT TRUE,
PRIMARY KEY(id)
);
COMMENT ON COLUMN "demo"."id" IS '主键';
COMMENT ON COLUMN "demo"."name" is '用户名';
COMMENT ON COLUMN "demo"."sex" is '性别：true 男，false 女，null 未知';
CREATE INDEX "index_name" ON demo USING btree ("name");
```
### 4 参考答案
```
ALTER TABLE demo ADD age int4 ;
COMMENT ON COLUMN "demo"."age" IS '年龄';
ALTER TABLE demo ALTER COLUMN age TYPE float8;
```
### 5.参考答案
```
--5.1
select * from person where city like '%jing%' or address like '%street%';
-- 5.2
使用通配符_
```
### 6 参考答案
```
-- 6.1 略
-- 6.2 >= ... <=
-- 6.3 文本，数字，日期
```
### 7.参考答案：
```
-- 7.1
LastName    FirstName   OrderNo
Adams   John    22456
Adams   John    24562
Carter  Thomas  77895
Carter  Thomas  
-- 7.2
LastName    FirstName   OrderNo
Adams   John    22456
Adams   John    24562
Carter  Thomas  77895
Carter  Thomas  
Bush    George   
-- 7.3
LastName    FirstName   OrderNo
Adams   John    22456
Adams   John    24562
Carter  Thomas  77895
Carter  Thomas  
                34764
```
### 8.参考答案
```
SELECT Customer,SUM(OrderPrice) FROM Orders
GROUP BY Customer
```
### 9.参考答案
```
-- 9.1
length(create_time) 和 to_char(create_time,'yyyy-MM-dd')
--9.2
2
3
5
```
### 10.
- delete和truncate只删除表的数据不删除表的结构
- 速度,一般来说: drop> truncate >delete
- delete语句是dml,这个操作会放到rollback segement中,事务提交之后才生效;如果有相应的trigger,执行的时候将被触发. truncate,drop是ddl, 操作立即生效,原数据不放到rollback segment中,不能回滚. 操作不触发trigger. 
- [参考连接1](http://blog.csdn.net/ws0513/article/details/49980547)

### 11.参考答案
```
order by create_time desc limit 10 offset 11;
```
### 12.参考答案
- 可以大大提高数据查询的速度在实现数据的参考完整性方面，可以加快表和表之间的连接在分组和排序时，可以减少分组和排序的时间
- 创建和维护索引需要耗费时间，数据量越大耗费时间也越长索引需要占用额外的磁盘空间，如果数据量很大又有大量索引，那么索引文件大小增加很快对表中数据改动的时候，索引需要动态维护，降低了数据操作的速度
- B-tree、Hash、GiST和GIN
### 13 
```
select b.ta_ysear,sum(a.ta_num) from "testA" as a ,"testA" as b
where a.ta_ysear <= b.ta_ysear GROUP BY(b.ta_ysear);
```
### 14.参考答案：
```
select 名称 from 商品表 group by 名称 having avg(进价) < 2
```

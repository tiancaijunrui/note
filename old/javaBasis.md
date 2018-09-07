---
title: javaBasis
date: 2017-02-22 18:40:35
tags: java
categories: java
---
### jdk环境配置
- JAVA_HOME:jdk安装路径 eg:
```
C:\Program Files (x86)\Java\jdk1.6.0_10
```
- path:
```
.;%JAVA_HOME%\bin
```
- classpath:
```
.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar
```
- 检测
```
cmd:控制台 java -version
eg：
java version "1.6.0_45"
Java(TM) SE Runtime Environment (build 1.6.0_45-b06)
Java HotSpot(TM) 64-Bit Server VM (build 20.45-b01, mixed mode)
```

### maven环境变量配置
- MAVEN_HOME：
```
E:\Maven\apache-maven-3.3.9
```
- path:
```
;%MAVEN_HOME%\bin
```
- 检测
```
cmd:控制台 mvn -v
eg:
Apache Maven 3.2.5 (12a6b3acb947671f09b81f49094c53f426d8cea1; 2014-12-15T01:29:23+08:00)
Maven home: D:\apache-maven-3.2.5\bin\..
Java version: 1.6.0_45, vendor: Sun Microsystems Inc.
Java home: C:\Program Files\Java\jdk1.6.0_45\jre
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 8", version: "6.2", arch: "amd64", family: "windows"
```

### Java 语言中 Enum 类型的使用介绍
- Enum类型的介绍
    + 5.0版本sdk发布的时候，在语言层面上增加了枚举类型。
- 定义
```
    -- 定义一个类
    public enum  EnumTest {
    TEST1,TEST2,TEST3;
    }
    -- 或者
    enum enumTest1 {test1 ,test2,test3,test4}
```
   
- 使用注意
    + enum类型不支持public和protected修饰符的构造方法，因此构造函数一定要是private或friendly的。<span style="color: red">也正因为如此，所以枚举对象是无法在程序中通过直接调用其构造方法来初始化的。</span>
    + 定义enum类型时候，如果是简单类型，那么最后一个枚举值后不用跟任何一个符号；但如果有定制方法，那么最后一个枚举值与后面代码要用分号‘;’隔开，不能用逗号或空格。
    + 由于 enum 类型的值实际上是通过运行期构造出对象来表示的，所以在 cluster 环境下，每个虚拟机都会构造出一个同义的枚举对象。因而在做比较操作时候就需要注意，如果直接通过使用等号 ( ‘ == ’ )操作符，这些看似一样的枚举值一定不相等，因为这不是同一个对象实例。

- Enum 相关工具类
    + EnumSet
    
    ```

    for(EnumTest enumTest1 : EnumSet.range(EnumTest.TEST1,EnumTest.TEST3)){
            System.out.println(enumTest1);
        }

        //输出
        TEST1
        TEST2
        TEST3

    ```

    + EnumMap
    
    ```
     EnumMap<EnumTest,EnumTest2> enumMap = new EnumMap<EnumTest, EnumTest2>(EnumTest.class);
        enumMap.put(EnumTest.TEST1,EnumTest2.TEST1_1);
        enumMap.put(EnumTest.TEST2,EnumTest2.TEST2_2);
        enumMap.put(EnumTest.TEST3,EnumTest2.TEST3_3);

        for (EnumTest enumTest1 : enumMap.keySet()){
            System.out.println(enumMap.get(enumTest1));
        }
        
        //输出
        TEST1_1
        TEST2_2
        TEST3_3

    ```

### java中的取整函数
- 向上
```
(int)Math.floor(NumberUtils.toDouble(str))
```
- 向下
```
(int)Math.ceil(NumberUtils.toDouble(str))
```
- 返回最接近参数的整数，如果有2个数同样接近，则会返回偶数的那个
```
(int)Math.rint(NumberUtils.toDouble(str))
```
- 四舍五入
```
Math.round(NumberUtils.toDouble(str))
```







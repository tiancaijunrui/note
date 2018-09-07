---
title: javaSkill
date: 2017-02-22 18:49:58
tags: java
categories: java
---
### 1. 两个数交换不用第三个数
```
a = a + b;
b = a - b;
a = a - b;
```

### 2.clone 克隆
- 浅拷贝
    + 一般就是直接调用对象的clone()方法，这样的拷贝，对于这个对象的基本数据类型（int,String,float等）的属性，是可以得到完全的拷贝，但是对于这个对象里面的<span style='color:red'>非基本</span>的数据类型(对象、集合等)的某些属性，实际上并没有进行克隆，而只是进行一个赋值操作而已，也就是说克隆出来的对象与原对象有一部分内容的内存地址是相同的。
	
```
    // 定义userDetail类
    public class UserDetail {
    private String idCard;

    public String getIdCard() {
        return idCard;
    }

    public void setIdCard(String idCard) {
        this.idCard = idCard;
    }

    @Override
    public String toString() {
        return "UserDetail{" +
                "idCard='" + idCard + '\'' +
                '}';
    }
}


    // 定义user类
    public class User implements Cloneable {
    private  String name;
    private Integer age;
    private UserDetail userDetail;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public UserDetail getUserDetail() {
        return userDetail;
    }

    public void setUserDetail(UserDetail userDetail) {
        this.userDetail = userDetail;
    }


    @Override
    public Object clone() throws CloneNotSupportedException {
        return (User)super.clone();
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
// 测试浅拷贝
    public static void main(String[] args) {
        User user = new User();
        user.setAge(18);
        user.setName("12");
        UserDetail ud = new UserDetail();
        ud.setIdCard("1234");
        user.setUserDetail(ud);
        try {
            User userCopy = (User) user.clone();
            user.setName("123");
            ud.setIdCard("333333");
            userCopy.getUserDetail().setIdCard("11111");
            System.out.println(user.toString());
            System.out.println(userCopy.toString());
            System.out.println(user.getUserDetail().getIdCard());
            System.out.println(userCopy.getUserDetail().getIdCard());
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
    }
    // 输出
    User{name='123', age=18}
    User{name='12', age=18}
    11111
    11111
```
- 深拷贝
    + 即覆写clone()方法，遍历对象中的每一个属性，采取合适的方法进行克隆即可，例如对于对象中的集合，可以遍历集合，然后构造一个新的集合，重新存储一遍。
```
    // 覆写user里面的clone方法
        User user = (User) super.clone();
        UserDetail userDetail = new UserDetail();
        userDetail.setIdCard(user.getUserDetail().getIdCard());
        user.setUserDetail(userDetail);
        return user;

        // 则输出为
        User{name='123', age=18}
        User{name='12', age=18}
        333333
        11111
```

### 3.file.delete() 与file.deleteOnExist() 方法区别
 - file.delete 是立刻将文件删除，不管文件存不存在，不抛出异常
 - file.deleteOnExist 是在服务停止的时候（不是代码块或者方法执行完的时候）执行删除文件的操作，相当于做了一个缓存，经常用作删除临时文件或临时目录

### 4.BeanUtils.copyProperties()与PropertyUtils.copyProperties()区别
- 导包：
```
import org.apache.commons.beanutils.BeanUtils;
import org.apache.commons.beanutils.PropertyUtils;
```
- BeanUtils.copyProperties()
```
1.通过反射将一个对象的值赋值给另外一个对象
2.两个参数a和b，是b的参数值赋值到a
3.
```

- 区别
	- PropertyUtils的copy方法提供类型转换功能，即两个javaBean的同名属性为不同类型时，在支持的数据类型范围内进行转换，PropertyUtils不支持这个功能，所以说BeanUtils速度会更快一些，使用更普遍一点，犯错的风险更低一点。
	 - BeanUtils支持的转换类型

 ```
    * java.lang.BigDecimal
    * java.lang.BigInteger
    * boolean and java.lang.Boolean
    * byte and java.lang.Byte
    * char and java.lang.Character
    * java.lang.Class
    * double and java.lang.Double
    * float and java.lang.Float
    * int and java.lang.Integer
    * long and java.lang.Long
    * short and java.lang.Short
    * java.lang.String
    * java.sql.Date
    * java.sql.Time
    * java.sql.Timestamp 
     // 这里要注意一点，java.util.Date是不被支持的，而它的子类java.sql.Date是被支持的。因此如果对象包含时间类型的属性，且希望被转换的时候，一定要使用java.sql.Date类型。否则在转换时会提示argument mistype异常。

 ```







    
---
title: 基于centos部署tomcatweb应用常用命令
date: 2017-04-19 18:21:17
tags: linux
categories: linux
---

### 1.重启linux命令
```
reboot
shutdown -r now 立刻重启（root用户使用）
shutdown -r 10 过十分钟自动重启
shutdown -r 20:35 在时间为20：35时候重启（root用户使用）
```

### 2.查看JAVA_HOME配置路径
```
echo $JAVA_HOME
```

### 3.启动tomcat和关闭tomcat进程的命令
```
cd ../bin
./startup.sh
./shutdown.sh 
```

### 4.显示当前路径
```
[root@vultr bin]# pwd
```
### 5. vi 的两种工作模式
    - 进入vi后为命令模式，按insert进入编辑模式
    - 按esc进入命令模式，在命令模式不能编辑只能输入命令
    ```
    :w 保存当前文档
    :q 直接退出
    :wq 先保存后退出
    :q! 强制不保存退出
    ```
#### 6.id hostname
```
    - id 显示
    [web@cloud1 ~]$ id
    uid=8888(web) gid=8888(web) groups=8888(web)
    [web@cloud1 ~]$ hostname
    cloud1

```
### 7.查看端口号是否被占用
```
-- 查看80端口是否被程序占用
    lsof -i :80
-- 如果 显示commond not found 错误 则需要执行
    yum install lsof -y 
```
### 8.查看历史执行命令和搜索命令
```
    history 查看历史命令
    !98 执行第98条命令
    ctrl+R 搜索命令，回车执行搜索出来的命令或者左右键调整后执行命令
```
### 9.文件浏览
```
ls 
ll -t 等价与 ls -l -t 按照时间排序
```
### 10 文件查看
```
head
tail
more
cat
grep  'key' filename
```
### 11.windows 控制台打包java项目
```
jar -cvf appName.war ./*
```

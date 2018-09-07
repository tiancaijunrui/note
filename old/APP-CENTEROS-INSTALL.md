---
title: app_centeros_install
date: 2018-05-10 20:16:06
tags: linux
---

# tc-parent

### 环境配置

- 安装postgresql

```
yum install https://download.postgresql.org/pub/repos/yum/9.5/redhat/rhel-7-x86_64/pgdg-centos95-9.5-2.noarch.rpm

yum install postgresql95-server postgresql95-contrib

systemctl enable postgresql-9.5.service

systemctl start postgresql-9.5.service

sudo su postgres 
psql postgres
ALTER USER postgres with PASSWORD 'zcj19921123'
\q
exit
vi /var/lib/pgsql/9.5/data/postgresql.conf
修改#listen_addresses = 'localhost'  为  listen_addresses='*'
// 防火墙配置略，腾讯云防火墙默认关闭
systemctl restart postgresql-9.5.service

查看命令：
ps aux | grep postgres
netstat -npl | grep postgres

重启命令：
#su - postgres
$pg_ctl restart

```

- nginx 安装

```
// 1 添加Nginx到YUM源
sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
// 2.安装Nginx
sudo yum install -y nginx
// 3.启动Nginx
sudo systemctl start nginx.service
// 4.CentOS 7 开机启动Nginx
sudo systemctl enable nginx.service

// 安装目录
/etc/nginx

sudo nginx -s reload
sudo service nginx restart
sudo nginx -s stop
```

- jdk 安装

```
// 新建文件夹
/usr/web/java/
// 将tar.gz 文件传到对应服务器
// 添加环境变量
export JAVA_HOME=jdk文件所在目录
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
// 执行命令
source /etc/profile

java -version
```

- redis-service 安装

```
yum install gcc make
// cd 至对应目录 eg：/usr/web/download/redis/
curl http://download.redis.io/releases/redis-4.0.9.tar.gz -o redis-4.0.9.tar.gz
tar zxvf redis-4.0.9.tar.gz
cd redis-4.0.9
make
cd utils/
./install_server.sh 
netstat –apn | grep 6379
```

- node安装

```
// 环境依赖
yum -y install gcc make gcc-c++ openssl-devel wget
wget https://nodejs.org/dist/v9.9.0/node-v9.9.0.tar.gz
// 解压
tar zxvf node-v9.9.0.tar.gz
cd node-v9.9.0
./configure
make
make install
// 查看版本
node -v

```

- 安装 MongoDB

```
vim /etc/yum.repos.d/mongodb.repo
以下内容：
------start------------
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
------end-------------

yum install mongodb-org

// 查看版本
mongod --version
mongo --version
mkdir ~/log
mongod --fork --logpath ~/log/mongodb.log
//检测是否启动
netstat -ltp | grep 27017

添加 MongoDB 用户
mongo
use weapp;
// user 和 pwd 正式环境要保密
db.createUser({ user: 'weapp', pwd: 'weapp-dev', roles: ['dbAdmin', 'readWrite']});
// 退出
exit
```

- 安装 Nodejs 模块

  - 包括 pm2 等 ,在项目根目录（/usr/web/services/websocket/race）下执行：

  ```
  npm install -g cnpm --registry=https://registry.npm.taobao.org

  cnpm i pm2 -g

  cnpm i
  ```

  - 其他常用命令

  ```
  pm2 start app.js
  pm2 stop app.js
  pm2 restart app.js
  // 查看日志
  pm2 logs
  //启动watch 模式
  pm2 start app.js --watch
  //停止watch 模式
  pm2 stop app.js --watch
  ```

  ​
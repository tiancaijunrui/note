---
title: linux
date: 2018-08-27 00:36:06
tags: linux
---
# linux

### 安装蓝灯

```bash
- 下载链接：https://raw.githubusercontent.com/getlantern/lantern-binaries/master/lantern-installer-preview-64-bit.deb
```

```bash
sudo apt install gdebi-core
```

```bash
sudo gdebi -lantern-installer-beta-64-bit.deb
```

### 安装golang

- 从官网下载最新的golang安装包
- 使用tar zxvf 包名 -C /usr/local 解压到指定目录
- 配置环境变量

```bash
sudo vim /etc/profile
```

```bash
添加：
export PATH=$PATH:/usr/local/go/bin
```

```bash
保存并退出：
source /etc/profile 使环境变量生效
```

```bash
验证是否安装完成：
go version 看是否安装完成
```

### deb 文件安装

```bash
// 安装
sudo dpkg -i deb文件名
```

```bash
// 安装依赖
sudo apt-get install -f
```

```bash
// 卸载
sudo dpkg -r 软件名
```

### 安装node

```bash
sudo apt install nodejs-legacy
sudo apt install npm
```

```bash
// 安装npm
sudo apt install npm
```

```bash
// 升级npm为最新版本
sudo npm install npm@latest -g
```

```bash
// 安装用于安装nodejs的模块n
sudo npm install -g n
```

```bash
// 安装官方最新版本
sudo n latest
// 安装官方稳定版本
sudo n stable
// 安装官方最新LTS版本
sudo n lts
```
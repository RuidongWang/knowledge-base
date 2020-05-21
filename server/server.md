# 服务器操作指南

> Tips: 防止遇到更多的坑，服务器建议选择 Ubuntu-18.04 （by Ruidong.Wang 2020）

## 1. 服务器刚到手的操作

**更新 linux 源**
```shell
sudo apt-get update
```
**升级**
```shell
sudo apt-get upgrade
```

## 安装 JAVA 8

```shell
sudo apt-get install openjdk-8-jdk
```

## 安装 mysql 5.7.30-0ubuntu0.18.04.1

**安装**
```shell
sudo apt install mysql-server
```

**更改密码**
```shell
sudo mysql_secure_installation
```

**启动 mysql**
```shell
systemctl start mysql
```

**查看运行状态**
```shell
systemctl status mysql
```

## 安装 nginx


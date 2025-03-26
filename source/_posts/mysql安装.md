---
title: mysql安装
date: 2022-11-03 20:18:37
tags: [mysql]
categories: [数据库]
---

### windows下安装mysql
[下载网站](https://dev.mysql.com/downloads/mysql/5.7.html#downloads)，选择zip下载,下载完成解压后把bin目录加入环境变量。

在根目录新建一个的ini文件，文件内配置：
<!--more-->
```ini
[mysql]
default-character-set=utf8#设置字符编码

[mysqld]
character-set-server=utf8
default-storage-engine=INNODB#设置默认引擎默认
```
打开cmd输入以下命令初始化MySQLmy ，初始化成功后根目录会生成data文件夹，用作存储数据。
```bash
mysqld --initialize-insecure
```


在cmd中输入, 安装mysal。
```bash
mysqld -install
```

输入，启动mysql服务
```bash
net start mysql
```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('root');
输入 ，设置root用户密码
```bash
mysql admin -u root password 1234
```

输入, 登录到mysql root用户

```bash
mysql -uroot -p1234
```

输入, 关闭mysql服务
```bash
net stop mysql 
```

卸载mysql 
先关闭mysql服务，然后输入，再删除环境变量与mysql安装目录
```bash
mysqld -remove mysql
```


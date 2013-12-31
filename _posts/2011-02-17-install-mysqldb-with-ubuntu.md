---
layout: post
title: install MySQLdb on ubuntu10.10 python2.6
date: 2011-02-17 11:35:20
categories: mysql
tags: python mysql 
---

两种方法：

1. 手动下载源码安装  --最新
2. 通过apt-get安装   --方便
 
两种方法的前提：<br>
安装`mysql_config`和`setuptools`：

  ```
  $sudo apt-get install libmysqlclient16-dev python-setuptools
  ```
 
动手阶段：<br>

1. 手动下载源码安装
  去`http://sourceforge.net/projects/mysql-python/`下载压缩包
  
  ```
   $tar xfz MySQL-python-x.x.x.tar.gz 
   $cd MySQL-python*
   $python setup.py build
   $sudo python setup.py install
 ```
 
2. apt-get安装

   ```
   $sudo apt-get install python-mysql
   ```
 
3. 安装验证: 
 
   ```
   $python
   >>>import MySQLdb
   >>>                           
   ```
没报错，说明成功。
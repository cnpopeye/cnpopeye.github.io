---
layout: post
title:  "[Win7]cx_Oracle 找不到指定模块错误解决"
date:   2010-08-06 17:24:46
categories: oracle
tags: python oracle
---

环境：
Windows7
Python 2.6.2
cx_Oracle 5.0.4
Oracle Instant Client 10.2.0

- a)常规方法解压Oracle Instant Client文件到指定目录
- b)设置环境变量
- c)设置tnsnames.ora
- d)pl/sql developer可以登录远程Oracle数据库

安装cx_Oracle，在python里面import cx_Oracle，提示：
{%highlight bash %}
>>> import cx_Oracle
Traceback (most recent call last):
File "<pyshell#0>", line 1, in <module>
import cx_Oracle
ImportError: DLL load failed: 找不到指定的模块。
{%endhighlight%}

网上搜索一番都在说复制oracle目录的`oci.dll`等文件到`$python_home/Lib/site-packages`即可。
反复尝试未果。
偶见一网文说复制`oci.dll`到`$python_home` （比如c:/python26），将信将疑操作后发现竟然可以了。
特此记录。
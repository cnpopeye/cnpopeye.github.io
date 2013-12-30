---
layout: post
title:  "cx_Oracle安装记录（CentOS5.3+Python2.4）"
date:   2010-08-06 15:14:46
categories: oracle
tags: python oracle
---
环境:
CentOS 5.3 64bit
Python 2.4
cx_Oracle-5.0.4-10g-py24-1.x86_64.rpm

###第一步： 安装Oracle客户端 --- Oracle instant client

- 1.到Oracle官网下载Oracle instant client文件包，建议下载zip,因为我们只需要里面几个库文件。
http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html

- 2.创建文件夹
{%highlight bash%}
$mkdir /opt/oracle/instantclient
{%endhighlight%}
- 3.复制库文件到instantclient目录
{%highlight bash%}
$cp instantclient_10_2/*.* /opt/oracle/instantclient
{%endhighlight%}

- 4.编辑环境变量
{%highlight bash%}
$nano /etc/profile

#文件尾部添加一下内容：
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/oracle/instantclient/
{%endhighlight%}

- 5.更新环境使生效
{%highlight bash%}
$source /etc/profile
{%endhighlight%}

### 第二步：安装cx_Oracle
1.下载cx_Oracle安装包，这里是CentOS，所以下载rpm；如果是deb系列，有两个方法：
a)源码编译安装；
b)使用alient转换后安装。
下载地址：http://cx-oracle.sourceforge.net/ 注意选择对应的Python版本。
2.rpm方式就简单了，安装rpm包
# rpm -iUvh oracle-instantclient-basic-10.2.0.4-1.x86_64.rpm
附加步骤：cx_Oracle 使用示例代码

{%highlight python linenos%}

'''

Created on 2010-1-21

@author: cn.popeye
'''
import cx_Oracle

class uDBO():

connection = cx_Oracle.connect("usrname","pwd","host:1521/SID")
cursor = connection.cursor()

def execQuerySQL( self,as_strsql):
    try:
        self.cursor.execute(as_strsql)
    except:
        print "查询失败",as_strsql
    return self.cursor

def connClose(self):
    self.cursor.close()
    self.connection.close()


if __name__ == "__main__":
    uo = uDBO()
    c = uo.execQuerySQL("SELECT name FROM users")
    for op in c.fetchall():
    print op

{%endhighlight%}


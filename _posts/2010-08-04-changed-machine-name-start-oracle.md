---
layout: post
title:  "修改机器名称后Oracle dbconsole无法启动!"
date:   2010-08-04 18:06:00
categories: oracle
---

修改机器名称后启动OracleDBConsole服务时出现提示“修改机器名称后Oracle无法启动”。
 
修改tnsnames.ora,listener.ora保证Oracle主服务可以使用。

但是一直不能EM管理，倒也无所谓；忽然又心血来潮非要把这问题搞定。

#####第一回合：

- 1.使用`emctl start dbconsole`，根据提示设置`ORACLE_SID`，复制需要的文件夹，还是提示找不到路径；
- 2.重建资料库，问题依旧。
- 3.跟踪`%ORACLE_HOME%/BIN/emctl.bat emctl.pl`脚本，发现还是使用
   `net start oracledbconsole<oraSID>`命令启动，未果。
 
#####第二回合：

- 1.找到网上某哥们的解决过程，前半部分第一回合第一步是一样的，问题未解决。
- 2.使用第二部分，emca 配置资料库，问题解决。
 
整理后步骤如下：
 
环境：
{% highlight bash%}
Windows 2003 ent 64bit
Oracle 10.2.0
test     // 原有的机器名  
test-123 // 新机器名 
{%endhighlight%}

###1.修改tnsnames.ora listener.ora保证oracle服务可用
 
打开并编辑`oracle/product/10.2.0/db_1/NETWORK/ADMIN`目录下的`tnsnames.ora`:
{% highlight bash%}
ORCL =
	(DESCRIPTION =
		(ADDRESS = (PROTOCOL = TCP)(HOST = test-123 )(PORT = 1521))
		(CONNECT_DATA =
			(SERVER = DEDICATED)
			(SERVICE_NAME = orcl)
		)
	)
{%endhighlight%}

 
 
打开并编辑`oracle/product/10.2.0/db_1/NETWORK/ADMIN`目录下的`listener.ora`:

{% highlight bash%}
LISTENER =
	(DESCRIPTION_LIST =
		(DESCRIPTION =
			(ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1))
			(ADDRESS = (PROTOCOL = TCP)(HOST = test-123 )(PORT = 1521))
		)
	)
{%endhighlight%}

* 启动数据库服务，启动监听服务没有出问题
* 此时启动OracleDBConsole还是出现问题


###2.启动OracleDBConsole服务 
手工从cmd命令行，并将当前目录为BIN所在目录，执行命令 
{% highlight bash%}
>emctl start dbconsole 
highlight bash%}Environment variable ORACLE_SID not defined. Please define it.  <<--提示错误
{%endhighlight%}

设置sid为你的数据库实例名称,这里是默认 `orcl`:
{% highlight bash%}
>set oracle_sid=orcl 
>emctl start dbconsole 
OC4J Configuration issue. F:/software/oracle/product/10.2.0/db_1/oc4j/j2ee/OC4J_DBConsole_test-123_orcl not found.
{%endhighlight%}

按错误提示找到`F:/software/oracle/product/10.2.0/db_1/oc4j/j2ee/`该目录并将`OC4J_DBConsole_test_orcl`文件夹名称修改为`OC4J_DBConsole_test-123_orcl`.

再次在cmd中执行出现错误:
{% highlight bash%}
>set oracle_sid=orcl 
>emctl start dbconsole
EM Configuration issue. F:/software/oracle/product/10.2.0 /db_1/test-123_orcl not found.
{%endhighlight%}

按错误提示找到`F:/software/oracle/product/10.2.0/db_1`该目录并将`test_orcl`文件夹名称修改为`test-123_orcl`
 
再执行命令出现错误
{% highlight bash%}
Oracle Enterprise Manager 10g Database Control Release 10.2.0.1.0
Copyright (c) 1996, 2005 Oracle Corporation. All rights reserved.
http://test-123:1158/em/console/aboutApplication
Starting Oracle Enterprise Manager 10g Database Control
...OracleDBConsoleorcl
服务正在启动 .
OracleDBConsoleorcl 服务无法启动。
系统出错。
系统发生 3 错误。
系统找不到指定的路径。
{%endhighlight%}
 
 
###3.重构 dbcontrol 配置文件-----------这部分解决问题
{% highlight bash%}
>emca -config dbcontrol db

EMCA 开始于 2010-6-12 11:28:24
EM Configuration Assistant 10.2.0.1.0 正式版
版权所有 (c) 2003, 2005, Oracle。保留所有权利。

输入以下信息:
数据库 SID: orcl
已为数据库 orcl 配置了 Database Control
您已选择配置 Database Control, 以便管理数据库 orcl
此操作将移去现有配置和默认设置, 并重新执行配置
是否继续? [yes(Y)/no(N)]: y
监听程序端口号: 1521
SYS 用户的口令:
DBSNMP 用户的口令:
SYSMAN 用户的口令:
通知的电子邮件地址 (可选):
通知的发件 (SMTP) 服务器 (可选):
-----------------------------------------------------------------

已指定以下设置

数据库 ORACLE_HOME ................ 

F:/software/oracle/product/10.2.0/db_1

数据库主机名 ................ test-123
监听程序端口号 ................ 1521
数据库 SID ................ orcl
通知的电子邮件地址 ...............
通知的发件 (SMTP) 服务器 ...............

-----------------------------------------------------------------
是否继续? [yes(Y)/no(N)]:y

2010-6-12 11:30:26 oracle.sysman.emcp.EMConfig perform
信息: 正在将此操作记录到 F:/software/oracle/product/10.2.0/db_1

/cfgtoollogs/emca
orcl/emca_2010-06-10_11-28-24-上午.log。
信息: 正在停止 Database Control (此操作可能需要一段时间)...
2010-6-12 11:30:39 oracle.sysman.emcp.util.DBControlUtil startOMS
信息: 正在启动 Database Control (此操作可能需要一段时间)...
2010-6-12 11:31:19 oracle.sysman.emcp.EMDBPostConfig 

performConfiguration
信息: 已成功启动 Database Control
2010-6-12 11:31:19 oracle.sysman.emcp.EMDBPostConfig 

performConfiguration
信息: >>>>>>>>>>> Database Control URL 为 http://test-123:1158/em 

<<<<<<<<<<<

已成功完成 Enterprise Manager 的配置
MCA 结束于 2010-6-12 11:31:19
{%endhighlight%}

备注 DBSNMP 与 SYSMAN 口令如果没有修改过为 默认为system
这个Oracle服务修改完毕。
现在还有个问题 通过 http://test-123:1158/em 访问时，用system用户进入
看到一般信息中主机名称没有改变，到现在也没找到原因 。
 
以后继续找吧
执行`emca`命令时
它会重新生成`oracle/product/10.2.0/db_1/sysman/config`，
`oracle/product/10.2.0/db_1/oc4j`, `/j2ee/OC4J_DBConsole`中的配置文件，
并由`oracle/product/10.2.0/db_1/oc4j/j2ee/OC4J_DBConsole`中文件生成
`oracle/product/10.2.0/db_1/oc4j/j2ee/OC4J_DBConsole_test-123_orcl/`中
所有的配置文件.

emca命令执行的日志`F:/software/oracle/product/10.2.0/db_1/cfgtoollogs/emca/orcl`
目录下有兴趣的可以看看，emca是怎么重构用户的过程。

监听日志:

- `/oracle/product/10.2.0/db_1/network/log/listener.log`
- `/oracle/product/10.2.0/db_1/network/trace/listener.trc`

原帖文章见[这里][origin_url]

[origin_url]: http://dev.firnow.com/course/7_databases/oracle/oraclejs/20100721/482910.html


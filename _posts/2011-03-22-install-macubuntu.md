---
layout: post
title: Macbuntu安装记事
date: 2011-03-22 13:59:20
categories: macubuntu
tags: ubuntu macubuntu 
---

【本文只有关键步骤，不是小白教程】
 
不想放弃linux，又眼馋Mac OS界面和操作便利，那么Macbuntu是个非常好的选择，它不是一个简单的主题换肤，加上全局菜单和docky，那就直接是Mac OS了。

以前也不太对Mac OS感兴趣，后来顿悟，绝对顿悟，很顺手的就用上了Macbuntu.安装比较简单，但是中间有写小问题会出现，以次记事备忘。
 
####下载安装macbuntu
   这部足够简单，去这里下载：http://sourceforge.net/projects/macbuntu/照着文档做即可。
 
   macbuntu安装后重启X会提示有组件不能使用是否删除： 那就删除吧 。
   
####如果安装后没有docky ，那么就这么干：

    ```
    sudo apt-get install docky  
    ```
   
####如果安装后没有全局菜单 ，那么就看下面：

1. 下载globalmenu安装包

	`http://code.google.com/p/gnome2-globalmenu/downloads/list`
 
   按照文档提示安装会出现找不到autugen文件问题. 可以下载一个老版本的，把autogen文件覆盖到新版解压后目录即可。
   
   下载地址：
   
   `http://github.com/gnome-globalmenu/gnome-globalmenu/archives/v0.7.8`
     
 
2. 安装必备的软件包

	```
	$sudo apt-get install intltool libtool libconf2-dev libgtk2.0-dev  
	$sudo apt-get install libwnck*  
	$sudo apt-get install libgnome-menu*  
	$sudo apt-get install libpanelapplet-*  
	$sudo apt-get install libnotify*  
	$sudo apt-get install xfce4-panel*  
	```
 
3. aclocal一下
 
   ```
    $aclocal  
   ```

4. 开始安装啦
 
   ```
    $./autogen.sh --prefix=/usr --sysconfdir=/etc --disable-tests --without-xfce4-panel     
    $make
    $export GTK_MODULES=globalmenu-gnome
    $sudo make install  
   ```

5. 重启X，在面板中右击增加全局菜单小组件即可。
 
这个时候，就变成Mac+Ubuntu了，几乎一模一样。操作方式也类似。

如果想更像一些，需要安装firefox和chrome的mac主题插件即可。

![screenshot]({{ site.url }}/img/install-macubuntu.png)
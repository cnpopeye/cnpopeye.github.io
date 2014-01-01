---
layout: post
title: PIL安装记录，编译支持jpeg png
date: 2011-11-06 14:02:20
categories: python
tags: python PIL 
---

PIL是python理想的图片处理module，但是想要良好的支持各种图片，还需要检查一下几步，否则会提示：`IOError: decoder jpeg not available之类的。`

我的环境：Linux mint 11 amd64 / Python2.7

####第一步：安装zlib png freetype   jpeg
1. install zlib (ubuntu 官方源没有zlib，别想apt-get了)

   下载zlib，(zlib.net已墙，可以去SF.net)，[http://sourceforge.net/projects/libpng/files/zlib/1.2.5/zlib-1.2.5.tar.gz/download?use_mirror=superb-dca2](http://sourceforge.net/projects/libpng/files/zlib/1.2.5/zlib-1.2.5.tar.gz/download?use_mirror=superb-dca2)
 
    ```
    $tar -xvzf zlib-1.2.5.tar.gz
    $cd zlib-1.2.5
    $./configure --prefix=/usr/local
    $make
    $sudo make install
    ```

2. install png（忘记apt-get吧）

    ```
    $ wget ftp://ftp.simplesystems.org/pub/libpng/png/src/libpng-1.5.6.tar.gz (如果文件不存在就浏览 /src/目录查找一下最新版)
    $ tar -xvzf libpng-1.5.6.tar.gz 
    $ cd libpng-1.5.6
    $ ./configure --prefix=/usr/local
    $ make 
    $ sudo make install
    ```
 
3. install freetype （忘记apt-get吧）

    ```
    $ wget http://nchc.dl.sourceforge.net/project/freetype/freetype2/2.4.7/freetype-2.4.7.tar.gz
    $ tar -xvzf freetype-2.4.7.tar.gz 
    $ cd freetype-2.4.7/
    $ ./configure --prefix=/usr/local
    $ make 
    $ make install
    ```
 
4. install jpeg （忘记apt-get吧）

    ```
    $ wget http://www.ijg.org/files/jpegsrc.v8c.tar.gz
    $ tar -xvzf jpegsrc.v8c.tar.gz
    $ cd jpeg-8c/
    $ ./configure --prefix=/usr/local
    $ make 
    $ sudo make install
    ```
 
 
#### 第二步：安装需要的 devel库（现在是想起apt-get的时候了）
 
    ```
    $sudo apt-get install libjpeg8-dev 
    $sudo apt-get install libpng12-dev
    $sudo apt-get install libfreetype6-dev
    $sudo apt-get install zlib1g-dev
    ```
 
#### 第三步：安装 PIL（ Python Imaging Library ）

    ```
    $ wget http://effbot.org/downloads/Imaging-1.1.7.tar.gz
    $ tar -xvzf Imaging-1.1.7.tar.gz
    $ cd Imaging-1.1.7/
    ```

    修改setup.py文件：

    ```
    $ nano setup.py
    ```
    
    修改如下： 
    ```
    JPEG_ROOT = "/usr/local/lib"
    ZLIB_ROOT = "/usr/local/lib"
    FREETYPE_ROOT = "/usr/local/lib"
    ```

    检查是否支持：
    ```
    $ python setup.py build_ext -i
    running build_ext
    --------------------------------------------------------------------
    PIL 1.1.7 SETUP SUMMARY
    --------------------------------------------------------------------
    version 1.1.7
    platform linux2 2.7.1+ (r271:86832, Apr 11 2011, 18:13:53)
    [GCC 4.5.2]
    --------------------------------------------------------------------
    *** TKINTER support not available
    --- JPEG support available         -----------------------> OK！ 
    --- ZLIB (PNG/ZIP) support available -----------------------> OK！ 
    --- FREETYPE2 support available ----------------------->  OK！ 
    *** LITTLECMS support not available
    --------------------------------------------------------------------
    To add a missing option, make sure you have the required
    library, and set the corresponding ROOT variable in the
    setup.py script.

    To check the build, run the selftest.py script.
    ```
    
   正式安装：

    ```
    $ python setup.py build
    $ sudo python setup.py install
    ```
    
#### 最后一步：验证
 
   ```
   #!/usr/bin/env python  
   # -*- coding:utf-8 -*-  


   import Image  
   picPath = '～/images/1212.jpg'  
  
   im = Image.open(picPath)  
   print im.getbbox()  
  
   输出结果图片尺寸：
   (0, 0, 600, 715)
   ```

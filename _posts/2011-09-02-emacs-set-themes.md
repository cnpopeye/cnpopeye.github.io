---
layout: post
title: Emacs主题设置
date: 2011-09-02 11:53:20
categories: emacs
tags: emcas  
---

###第一招：使用color-theme现有主题
 
#####安装color-theme

下载地址：http://download.gna.org/color-theme/

下载解压缩后：

1. 把color-theme.el拷贝到某个load-path中，比如~/.emacs.d/lisp
2. 把themes目录也要拷贝到上面的路径中
3. 在~/.emacs里添加下面几行：
 
    ```
    (require ‘color-theme)  
    (color-theme-initialize)  
  
    ;;自己选择的一个主题，可以换成自己喜欢的  
    (color-theme-oswald)  
    ```
 用`M-x color-theme-select`可以选择主题，中意的就把第三行替换成那个。
 
 
###第二招：自己配置主题（ 必须先安装color-theme）
 
 
1. 在线配置主题

   在[http://alexpogosyan.com/color-theme-creator](http://alexpogosyan.com/color-theme-creator) 可以根据自己喜好，配置主题颜色。
  
   生成的.el文件存下来放到emacs load-path 目录中。别忘及修改“your-config-name-here”为你喜欢的名字比如`color-theme-mytheme`。
 
2. 然后再修改~/.emacs

    ```
    ;;下面这两行与第一招一样  
    (require ‘color-theme)     
    (color-theme-initialize)    
    ;;亮点在第三行  
    (color-theme-mytheme)  
    ```
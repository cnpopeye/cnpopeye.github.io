---
layout: post
title: 让Emacs ECB支持鼠标
date: 2011-09-02 10:24:20
categories: emacs
tags: emcas python 
---

ECB模式开启四个窗口，可以浏览目录，源码文件，方法和历史记录，但是默认只支持键盘操作，RET才能打开。
要支持鼠标需如下操作：

1. 在 Emacs 中执行“ M-x ecb-customize-most-important ”，找到“ Ecb Primary Secondary Mouse Buttons ”选项
2. 将其设为“ Primary: mouse-1, secondary: mouse -2 ” ，
3. 点击state，设置成“ Save for Future Sessions ”保存。
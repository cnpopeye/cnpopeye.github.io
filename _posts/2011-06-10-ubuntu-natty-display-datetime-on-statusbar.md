---
layout: post
title: Ubuntu Natty 11.04 解决中文托盘日期无法显示的问题
date: 2011-06-10 10:47:20
categories: ubuntu
tags: ubuntu linux 
---

原文参考：[http://tech.ranmocy.info/2011/04/ubuntu-natty-1104.html](http://tech.ranmocy.info/2011/04/ubuntu-natty-1104.html)
 
 
Ubuntu Natty 11.04中，右上角的托盘在设定的时候，如果开启了显示日期，就会使得整个日期显示托盘消失。只能在系统设置中重新修改。

问题的原因在于在选择显示日期的时候，设置显示的格式中有些选项在中文环境下显示有问题，系统无法格式化字符串，导致整个时间都无法显示。我们可以自己手动修改系统配置。

请大家使用GUI方式。
#####Terminal方式第二条命令失败，返回"0:expected value"，请高手解释。 

###Terminal方式：
```
$gsettings set com.canonical.indicator.datetime time-format "custom"
$gsettings set com.canonical.indicator.datetime custom-time-format "%F %A %R:%S"
```

###GUI方式：
安装dconf-tools：  
```$sudo apt-get install dconf-tools```

运行dconf-tools:   
```$dconf-etor    #（新版是dconf-editor,而不是原文说的dconf-tools）```

依次展开`com/canonical/indicator/datetime`

在右侧修改`time-format`字段为`"custom"`，修改`custom-time-format`字段为`"%F %A %R:%S"`。

其中custom-time-format字段的格式可以自己随意更改，详情请参考`man strftimez`。

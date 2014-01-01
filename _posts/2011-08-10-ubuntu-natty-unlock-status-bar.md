---
layout: post
title: Ubuntu Linux 11.04 Natty解除系统托盘限制
date: 2011-08-10 15:52:20
categories: mysql
tags: python mysql 
---

在 Ubuntu 11.04 Natty 中，Ubuntu 对顶部面板右上角的通知区域（系统托盘）采用了白名单制度，只有支持 Indicators 并位于白名单的部分程序才会被显示在系统托盘中，目前支持的程序有： Java apps, Mumble, Wine applications, Skype 和 hp-systray 。除些之外的 DropBox ，aMule 等大量程序都不会被显示在系统托盘中，不过事实上我们可以通过以下方法来解除这一限制。

- terminal方式 ：

解禁所有程序

```
$gsettings set com.canonical.Unity.Panel systray-whitelist "['all']"
```
或者只解禁部分程序，把 `YOUR_APPLICATION` 替换成你需要解禁的程序。

```
$gsettings set com.canonical.Unity.Panel systray-whitelist "['JavaEmbeddedFrame', 'Mumble', 'Wine', 'Skype', 'hp-systray', 'YOUR_APPLICATION']"
```

- GUI 方式 ：

安装 `dconf-tools`
```
sudo apt-get install dconf-tools
```

在终端中输入 `dconf-editor` ，然后找到desktop > unity > panel ，把 `systray-whitelist` 的值改为 `['all']` 。

最后注销并重新登录就可以了。

###还原
- 命令

```
$gsettings set com.canonical.Unity.Panel systray-whitelist "['JavaEmbeddedFrame', 'Mumble', 'Wine', 'Skype', 'hp-systray']"
```

or

- GUI 下在 `dconf-editor` 中点击 `Set to default` 按钮。	
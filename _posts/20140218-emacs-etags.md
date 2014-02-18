---
layout: post
title: "Emacs etags使用"
date: 2014-02-18 09:25:18
categories: emacs
tags: emacs etags
---

还是备忘一下。

要把emacs打造成称手的工具少不了查找引用和定义跳转等。
etags是非常小巧灵活的方式。

此处记录简单实用，若详详细了解可参考：http://www.emacswiki.org/emacs/EmacsTags

第一步：生成TAGS

```
   M-! etags *.py
```

第二步：载入TAGS

```
   M-x visit-tags-table TAGS-filename
```

第三步：使用

- M-. ：跳至相应的定义处；
- M-* ：返回刚才的定义处；
- C-u M-. ：查找下一个tags

---
layout: post
title: .emacs 配置
date: 2011-04-21 21:35:20
categories: mysql
tags: emacs 
---


```
;;设置自己的site-lisp载入路径
(setq load-path
(cons (expand-file-name "/home/zc/.emacs.d/lisp/") load-path))


;;设置缺省模式是text，而不是基本模式
(setq default-major-mode 'text-mode)

;;不显示工具栏
(tool-bar-mode nil)

;;与其他程序互相能copy/paste
(setq x-select-enable-clipboard t)

;;把title设置为 文件名@Emacs
(setq frame-title-format "%b@Emacs")

;;显示列号 在下面的工具栏加上行数显示
(setq column-number-mode t)

;;左边显示行号
(require 'linum)
(global-linum-mode 1)

;设置默认的列数是110
(setq default-fill-column 110)

;;设置kill-ring-max为200
(setq kill-ring-max 200)


;;设置tab为4个空格的宽度，而不是原来的2
(setq c-basic-offset 4)
(setq default-tab-width 4)

(add-hook 'text-mode-hook 'turn-on-auto-fill)
(global-font-lock-mode t)

;;下面的这个设置可以让光标指到某个括号的时候显示与它匹配的括号
(show-paren-mode t)
(setq show-paren-style 'parentheses)

;;显示日期
(setq display-time-day-and-date t)
(display-time)

;;显示时间
(setq display-time-24hr-format t)
(setq display-time-day-and-date t)
(setq display-time-use-mail-icon t)
(setq display-time-interval 10)

;;防止页面滚动时跳动
(setq scroll-margin 3
scroll-conservatively 10000)

;;'y' for 'yes', 'n' for 'no'
(fset 'yes-or-no-p 'y-or-n-p) 

;; 代码折叠
(load-library "hideshow")
(add-hook 'java-mode-hook 'hs-minor-mode)
(add-hook 'perl-mode-hook 'hs-minor-mode)
(add-hook 'php-mode-hook 'hs-minor-mode)
(add-hook 'emacs-lisp-mode-hook 'hs-minor-mode)

;; 如果设置为 t，光标在 TAB 字符上会显示为一个大方块 
(setq x-stretch-cursor nil)

;;tabbar
(require 'tabbar)
(tabbar-mode t)
(define-prefix-command 'lwindow-map)
(global-set-key (kbd "<M-up>") 'tabbar-backward-group)
(global-set-key (kbd "<M-down>") 'tabbar-forward-group)
(global-set-key (kbd "<M-left>") 'tabbar-backward)
(global-set-key (kbd "<M-right>") 'tabbar-forward)

;;把除了Emacs Buffer之外的文件都放成一组 默认是将后缀相同的放在一个组
;(setq tabbar-buffer-groups-function
; (lambda (b) (list "All Buffers")))
;(setq tabbar-buffer-list-function
; (lambda ()
; (remove-if
; (lambda(buffer)
; (find (aref (buffer-name buffer) 0) " *"))
; (buffer-list))))


;;session
(require 'session)
(add-hook 'after-init-hook
'session-initialize)


;; HACK: 要放在最后,免得会出现比较奇怪的现象
;; 保存和恢复工作环境
;; desktop,用来保存Emacs的桌面环境 — buffers、以及buffer的文件名、major modes和位置等等
(desktop-save-mode 1)

;;google maps
;;http://julien.danjou.info/google-maps-el.html
(require 'google-maps)
```
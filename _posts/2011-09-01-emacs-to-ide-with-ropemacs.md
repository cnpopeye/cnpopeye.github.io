---
layout: post
title: 踢开Eclisep&Aptana，Emacs变身强大IDE
date: 2011-09-01 17:26:20
categories: emacs
tags: emcas python 
---


需要用的到的：

1. emacs   --宿主
2. yasnippet  --模板工具，灰常好用，输入class 按TAB，就会自动生成class的模板
3. pymacs    
4. rope         --rope开头的是非常棒的重构工具，比如rename,move,extract method等等。还有非常好用的goto difinition(跳到定义)，show documents(显示文档)等等。
5. ropemacs
6. ropemode
7. auto-complete  --自动补齐功能，一会儿看截图
8. pycomplete       --也是自动补齐
9. cedete              --CEDET is a C ollection of E macs D evelopment E 10. nvironment Tools
10. ecb                   --emacs code browser，直接当作IDE文件浏览功能
 
###动手吧

本来在ubuntu 11.04下，安装了一些插件，但是用起来不是很爽，加上我把插件都放在`~/.emace.d`这个地方，因此决定手动安装吧。

1. install emacs
   
   ``` 
    $sudo apt-get install emacs  
   ```
   
2. install yasnippet

   下载地址：http://code.google.com/p/yasnippet/downloads/list
    `~/.emacs` 配置：
    
    ```
    ;;yasnippet  
    (require 'yasnippet-bundle)   
    (yas/initialize)   
    (yas/load-directory "~/.emacs.d/lisp/yasnippet-read-only/snippets")  
    ```
    
3. install pymacs

   下载地址：http://pymacs.progiciels-bpi.ca/pymacs.html

   ```
    $make install <pymacs 0.24-beta2 >  --指定python版本可使用下面这个命令  
    $ make install  PYTHON=python2.6  
    $sudo python setup.py install    
   ```
   ~/.emacs 配置：   
 
   ```
    ;;pymacs  
    (autoload 'pymacs-apply "pymacs")  
    (autoload 'pymacs-call "pymacs")  
    (autoload 'pymacs-eval "pymacs" nil t)  
    (autoload 'pymacs-exec "pymacs" nil t)  
    (autoload 'pymacs-load "pymacs" nil t)  
   ```

4. install rope

   下载地址：http://pypi.python.org/pypi/rope
 
    安装命令：
    ```
      $sudo python setup.py install  
    ```

5. ropemacs
   下载地址：https://github.com/pinard/Pymacs/downloads
 
    安装命令：
    
    ```
    $sudo python setup.py install  
     ```
     
   ~/.emacs 配置：   


    ```
    ;;repomacs  
    (pymacs-load "ropemacs" "rope-")  
    (setq ropemacs-enable-autoimport t)  
    ```
    
6. install ropemode
   下载地址：http://pypi.python.org/pypi/ropemode
 
   安装命令：
   
    ```
    $sudo python setup.py install  
    ```

7. install auto-complete
   下载地址：http://cx4a.org/software/auto-complete/#Downloads
    ~/.emacs 配置：   


    ```
    ;;auto-complete  
    (add-to-list 'load-path "~/.emacs.d/lisp/auto-complete-1.3.1")    
    (require 'auto-complete)  
    (require 'auto-complete-config)  
  
    (add-to-list 'ac-dictionary-directories 
                "~/.emacs.d/lisp/auto-complete-1.3.1/dict")    
    (ac-config-default)  
  
    (global-auto-complete-mode t)  
  
    ;(setq-default ac-sources '(ac-source-words-in-same-mode-buffers))  
    (setq-default ac-sources '(ac-source-yasnippet    
                 ac-source-semantic  
                 ac-source-ropemacs  
                 ac-source-imenu    
                 ac-source-words-in-buffer  
                 ac-source-dictionary  
                 ac-source-abbrev    
                 ac-source-words-in-buffer    
                 ac-source-files-in-current-dir    
                 ac-source-filename))   
  
    (add-hook 'emacs-lisp-mode-hook    
        (lambda () (add-to-list 
                    'ac-sources 
                    'ac-source-symbols)))  
    (add-hook 'auto-complete-mode-hook 
            (lambda () (add-to-list 
                    'ac-sources 
                    'ac-source-filename)))  
                    
    ;;下面这句是从auto-complete-config.el中翻出来的  
    ;;加上这句，在python中输入类的 . 就可以提示里面的方法了  
    (add-hook 'python-mode-hook        
        (lambda () (add-to-list '
            ac-omni-completion-sources 
            (cons "\\." '(ac-source-ropemacs)))  ))    
  
  
    (set-face-background 'ac-candidate-face "lightgray")  
    (set-face-underline 'ac-candidate-face "darkgray")  
    (set-face-background 'ac-selection-face "steelblue")   
  
    (setq ac-auto-start 2)  
    (setq ac-dwim t)  
     ```

8. pycomplete
   下载地址：
     - http://www.rwdev.eu/python/pycomplete/pycomplete.el 
     - http://www.rwdev.eu/python/pycomplete/pycomplete.py
   
   安装：
     a. `pycomplete.el`放到emacs加载目录
     b. `pycomplete.py`放到PYTHONPATH，如：/usr/local/lib/python2.6/dist-packages
     c. ~/.emacs 配置：  
    
    ```
    (require 'pycomplete)  
    (setq auto-mode-alist (cons '("\\.py$" . python-mode) auto-mode-alist))  
    setq interpreter-mode-alist(cons '("python2.6" . python-mode)  
                           interpreter-mode-alist))  
  
    (setq py-python-command "python2.6")   ;;这是我指定pythyon版本  
    (autoload 'python-mode "python-mode" "Python editing mode." t)  
    ```
    
9. cedete

   下载地址：http://sourceforge.net/projects/cedet/
    ~/.emacs 配置：  
    
    ```
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;  
    ;;      Cedet 1.0  
    ;;  
    (load-file "~/.emacs.d/lisp/cedet-1.0/common/cedet.el")  
      (global-ede-mode 1)                      ; Enable the Project management system  
      (semantic-load-enable-code-helpers)      ; Enable prototype help and smart completion   
      (global-srecode-minor-mode 1)            ; Enable template insertion menu  
    ;  
    ;;  
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;  
    ```
    
10. ecb
   
    下载地址：http://ecb.sourceforge.net/
    
    ~/.emacs 配置：

    ```
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;  
    ;;      ECB 2.40  
    ;;  
  
    (add-to-list 'load-path  
                    "~/.emacs.d/lisp/ecb-2.40")  
  
    (require 'ecb)  
    (require 'ecb-autoloads)  
  
    ;; ;;;;窗口间切换  
    (global-set-key [M-left]  'windmove-left)  
    (global-set-key [M-right] 'windmove-right)  
    (global-set-key [M-up]    'windmove-up)  
    (global-set-key [M-down]  'windmove-down)  
  
     ;;;;show&hide window  
    (global-set-key [C-f1] 'ecb-hide-ecb-windows)  
    (global-set-key [C-f2] 'ecb-show-ecb-windows)  
  
  
    ;; ;;;; 使某一ecb窗口最大化  
    (global-set-key (kbd "C-c 1") 'ecb-maximize-window-directories)  
    (global-set-key (kbd "C-c 2") 'ecb-maximize-window-sources)  
    (global-set-key (kbd "C-c 3") 'ecb-maximize-window-methods)  
    (global-set-key (kbd "C-c 4") 'ecb-maximize-window-history)  
  
    ;; ;;;;恢复原始窗口布局  
    (global-set-key (kbd "C-c 0") 'ecb-restore-default-window-sizes)  
  
    ;;  
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;  
    ```
 
有图有真相：
![shootscreen]({{site.url}}/img/emacs-ide-with-ropemacs.png)
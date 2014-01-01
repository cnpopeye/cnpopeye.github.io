
---
layout: post
title: ubuntu+uwsgi+nginx+web.py安装配置记录
date: 2012-05-18 13:45:26
categories: uwsgi
tags: ubuntu uwsgi nginx install
desc: install uwsgi、nginx and web.py into ubuntu
---

install:

###install depends packages
```
$sudo apt-get install python-dev
$sudo apt-get install python-webpy 
$sudo apt-get install nginx uwsgi-extra
```

###install uwsgi
```
$sudo apt-get install libxml2
$hg clone http://projects.unbit.it/hg/uwsgi-1.2
$cd /uwsgi-1.2
$python uwsgiconfig.py --build
$python setup.py install
```

***:$ ubuntu 仓库的uwsgi貌似版本有问题，会提示-w参数无效 or --module 参数无效
  
###configure nginx

edit file: `/etc/nginx/sites-enabled/myapp`:

```
server {  
        listen   80; ## listen for ipv4; this line is default and implied  
        server_name localhost;  
  
        root   /data/www/myapp;  
        index  index.html index.htm;  
  
        location / {  
                include uwsgi_params;  
                uwsgi_pass 127.0.0.1:9001;  
        }  
  
  
        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {  
                expires      30d;  
        }  
  
        location ~ .*\.(js|css)?$ {  
                expires      1h;  
        }  
  
        # redirect server error pages to the static page /50x.html  
        error_page   500 502 503 504  /50x.html;  
        location = /50x.html {  
                root   /var/www/nginx-default;  
        }  
  
        # deny access to .htaccess files, if Apache's document root  
        location ~ /\.ht {  
                deny  all;  
        }  
```

###configure uwsgi
 
edit file: `/etc/uwsgi/apps-enabled/myapp.xml`:

(you should do rename `myapp.xml` to your `appname.xml`)

```
<uwsgi id="myapp">  
  <socket>127.0.0.1:9001</socket>  
  <module>myapp</module>  
  <master />  
  <pythonpath>/data/www/myapp</pythonpath>  
  <chdir>/data/www/myapp</chdir>  
  <processes>2</processes>  
  <workers>4</workers>  
  <memory-report/>  
  <pidfile>/tmp/uwsgi_myapp.pid</pidfile>  
  <max-requests>10000</max-requests>  
  <daemonize>/var/log/uwsgi_myapp.log</daemonize>   
</uwsgi>  
```
 
 
 
```
#start uwsgi:  
$ uwsgi -x /etc/uwsgi/apps-enabled/myapp.xml  

# stop uwsgi:  
$ uwsgi --stop  
  or  
$ kill -SIGINT `cat /tmp/uwsgi_myapp.pid`  

#reload uwsgi:  
$ uwsgi --reload  
  or  
$ kill -SIGHUP `cat /tmp/uwsgi_myapp.pid`  

#pause uwsgi:  
$ uwsgi --pause  
  or  
$ kill -SIGTSTP `cat /tmp/uwsgi_myapp.pid`  
 
#suspend uwsgi:  
$ uwsig --suspend  
  or  
$ kill -SIGTSTP `cat /tmp/uwsgi_myapp.pid`  

#resume uwsgi:  
$ uwsgi --resume  
  or  
$ kill -SIGTSTP `cat /tmp/uwsgi_myapp.pid`  
```
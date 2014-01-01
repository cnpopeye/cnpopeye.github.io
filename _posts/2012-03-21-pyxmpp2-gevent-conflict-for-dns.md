---
layout: post
title: pyxmpp2和gevent.monkey.patch_socket关于dns解析方法冲突的解决
date: 2012-03-21 18:49:26
categories: xmpp
tags: xmpp gevent pyxmpp2
desc: pyxmpp2和gevent.monkey.patch_socket关于dns解析方法冲突的解决
---

最近在做xmpp相关项目。使用gevent和pyxmpp2。多进程+gevent协程，效果还是不错。 

但是打上patch_socket()就会出现问题，如果SRV域名的A记录已存在的话，将会直接解析起A记录。 


举个例子： 

```
SRV:_xmpp-client._TCP.gmail.com SRV 20 0 0 talk.l.gmail.com 
A:gmail.com  A 79.18.125.19 
```

当`patch_socket()`后，就会直接解析gmail.com A记录到 `79.18.125.19`，而不是解析`talk.l.gmail.com`. 而gmail.com并没有`5222`端口提供xmpp服务，因此消息会发送失败。 


阅读pyxmpp2源码和gevent文档发现： 

`pyxmpp2.transport._connect()`中解析地址会先`flag=socket.AI_NUMERICHOST` 强制使用IP地址方式，失败之后才会尝试SRV解析。 

而gevent.socket文档明确说明getaddrinfo会忽略flag参数： 

```
Differs in the following ways: 

raises DNSError (a subclass of gaierror) with libevent-dns error codes instead of standard socket error codes 
flags argument is ignored 
for IPv6, flow info and scope id are always 0 
```

因此，patch_socket()且gmail.com存在A记录的时候，gevent.socket将会解析成功，并且返回使用。 


解决方法： 

```
import gevent 
from gevent import monkey 
monkey.patch_socket( dns = False, aggressive = True ) 
```

patch时加`dns=False`，不使用`gevent.socket`的dns相关方法，使pyxmpp2.transport中connect函数flag=socket.AI_NUMERICHOST生效，方法包括： 

```
__dns__ = ['getaddrinfo', 
           'gethostbyname', 
           'gethostbyname_ex', 
           'gethostbyaddr', 
           'getnameinfo', 
           'getfqdn'] 
```

一般来说并不会影响patch_socket性能,毕竟只是dns解析还是使用python.socket而已。 

特此分享，不走弯路。
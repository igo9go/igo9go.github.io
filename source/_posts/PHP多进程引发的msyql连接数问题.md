---
title: PHP多进程引发的msyql连接数问题
date: 2016-09-07 14:45:47
tags:
categories:
---

# PHP多进程引发的msyql连接数问题

 业务中有一块采用了PHP的pcntl_fork多进程，希望能提高效率，但是在执行的时候数据库报错
 ```
 PDO::prepare(): Premature end of data (mysqlnd_wireprotocol.c:1244)
Packets out of order. Expected 1 received 108. Packet size=7102829
```
发现应该是短时间大量的链接写入数据库.导致数据库无法响应

show variables like '%max_connections%';
show variables like '%back_log%';

修改my.ini 配置 back_log
back_log = 104
MySQL能有的连接数量。当主要MySQL线程在一个很短时间内得到非常多的连接请求，这就起作用，
然后主线程花些时间(尽管很短)检查连接并且启动一个新线程。back_log值指出在MySQL暂时停止回答新请求之前的短时间内多少个请求可以被存在堆栈中。
如果期望在一个短时间内有很多连接，你需要增加它。也就是说，如果MySQL的连接数据达到max_connections时，新来的请求将会被存在堆栈中，
以等待某一连接释放资源，该堆栈的数量即back_log，如果等待连接的数量超过back_log，将不被授予连接资源。
另外，这值（back_log）限于您的操作系统对到来的TCP/IP连接的侦听队列的大小。
你的操作系统在这个队列大小上有它自己的限制（可以检查你的OS文档找出这个变量的最大值），试图设定back_log高于你的操作系统的限制将是无效的。


 在linux中，/proc/sys/net/core/somaxconn这个参数，linux中内核的一个不错的参数somaxconn
　　看下其解析：
　　对于一个TCP连接，Server与Client需要通过三次握手来建立网络连接.当三次握手成功后,
　　我们可以看到端口的状态由LISTEN转变为ESTABLISHED,接着这条链路上就可以开始传送数据了.
　　每一个处于监听(Listen)状态的端口,都有自己的监听队列.监听队列的长度,与如下两方面有关:
　　- somaxconn参数.
　　- 使用该端口的程序中listen()函数.
　　1. 关于somaxconn参数:
　　定义了系统中每一个端口最大的监听队列的长度,这是个全局的参数,默认值为128,具体信息为:
　　Purpose:
　　Specifies the maximum listen backlog.
　　Values:
　　Default: 128 connections
　　Range: 0 to MAXSHORT
　　Type: Connect
　　Diagnosis:
　　N/A
　　Tuning
　　Increase this parameter on busy Web servers to handle peak connection rates.
　　看下FREEBSD的解析：
　　限制了接收新 TCP 连接侦听队列的大小。对于一个经常处理新连接的高负载 web服务环境来说，默认的 128 太小了。大多数环境这个值建议增加到 1024 或者更多。 服务进程会自己限制侦听队列的大小(例如 sendmail(8) 或者 Apache)，常常在它们的配置文件中有设置队列大小的选项。大的侦听队列对防止拒绝服务 DoS 攻击也会有所帮助。
   我们可以通过，
    echo 1000 >/proc/sys/net/core/somaxconn
    修改连接数


Mac 环境修改如下：
增加max open files。先查看一下:
$ sysctl -a | grep files
kern.maxfiles = 12288 kern.maxfilesperproc = 10240

修改为
$ sudo sysctl -w kern.maxfiles=65535 $ sudo sysctl -w kern.maxfilesperproc=65535

增加 max sockets
$ sysctl -a | grep somax

kern.ipc.somaxconn: 128 $ sudo sysctl -w kern.ipc.somaxconn=2048

    


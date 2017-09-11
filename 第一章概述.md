# 内部实现概述

先上一个从easy-swoole挪过来的结构图

![](http://static.zybuluo.com/Lancelot2014/xpatz2wxco47xrzi5xc3keni/structure.png)

启动的时候由master进程创建manager进程，再由manager进程派生出多个worker进程，

之后master进程根据配置创建指定个数的Reactor线程，Reactor线程监听对应worker进程的管道，

master进程使用epoll\(linux下\)监听指定端口，当有客户端请求时，获取连接描述符，并交由Reactor线程来获取客户端发送的数据

Reactor线程接收到数据后，会根据一定的分配规则，为该请求分配一个worker进程，并写入该进程监听的管道中

在worker进程执行结束后，将结果写入自身的管道中，由监听该进程管道的reactor线程接收，并发送给客户端


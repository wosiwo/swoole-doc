```
PHP_METHOD(swoole_server, start)->swServer_start->
#swServer_start函数有两个分支
1. factory->start->swFactoryProcess_start 循环创建worker进程


2 .swServer_start_proxy->swReactorThread_start->swReactorThread_loop 循环创建reactor线程

其中main_reactor监听端口，有连接进来后讲连接fd绑定到某个reactor线程的epoll循环中

随后在main_reactor中销毁改连接，继续等待下一个连接

本次的连接则由reactor线程接管，接收到数据后抛给worker进程，()



Swoole暴露给php用于启动的类是swoole_server

其中核心的三个方法是
    1. PHP_METHOD(swoole_server, set) 用于设置启动参数,e.g worker_num设置worker进程数量
    2. PHP_METHOD(swoole_server, on)
```




PHP\_METHOD\(swoole\_server, start\)-&gt;swServer\_start-&gt;

\#swServer\_start函数有两个分支

1. factory-&gt;start-&gt;swFactoryProcess\_start 循环创建worker进程

2 .swServer\_start\_proxy-&gt;swReactorThread\_start-&gt;swReactorThread\_loop 循环创建reactor线程

其中main\_reactor监听端口，有连接进来后讲连接fd绑定到某个reactor线程的epoll循环中

随后在main\_reactor中销毁改连接，继续等待下一个连接

本次的连接则由reactor线程接管，接收到数据后抛给worker进程，\(\)

\#Swoole暴露给php用于启动的类是swoole\_server

其中核心的三个方法是

1. PHP\_METHOD\(swoole\_server, set\) 用于设置启动参数,e.g worker\_num设置worker进程数量

2. PHP\_METHOD\(swoole\_server, on\) 设置事件的回调函数 最主要的是onReceive事件的回调，其实就是接受到客户端请求swoole所调用的php代码

3. PHP\_METHOD\(swoole\_server, start\)  启动服务，创建worker进程，绑定端口，创建reactor线程等操作让服务器完成启动

在php执行启动代码

```
$this->sw->start();
```




```
PHP_METHOD(swoole_server, start)->swServer_start->
#swServer_start函数有两个分支
1. factory->start->swFactoryProcess_start 循环创建worker进程


2 .swServer_start_proxy->swReactorThread_start->swReactorThread_loop 循环创建reactor线程
```




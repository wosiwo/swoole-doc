# 内部实现概述

先上一个从easy-swoole挪过来的结构图

![](http://static.zybuluo.com/Lancelot2014/xpatz2wxco47xrzi5xc3keni/structure.png)

启动的时候由master进程创建manager进程\(待确认启动的那个进程到底是master还是manager\)，并根据配置创建指定个数的Reactor线程，与客户端的连接建立后，由Reactor线程来监听客户端发送的数据

Reactor线程接收到数据后，抛给manager进程的Factory模块\(待确认\),Factory会根据一定的分配规则，为该请求分配一个worker进程，并在worker进程执行结束后，将结果返回客户端



```c
    #define SW_SHM_MMAP_FILE_LEN  
```






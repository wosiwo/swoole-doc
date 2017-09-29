# epoll详解

## 功能描述

epoll 在[基础知识](../append/F3_基础知识.md)里已经讲解了.代码方法是一套组合方法主要有一下方法.

## 方法

* epoll_create:用来创建epoll实例,成功返回epoll实例的文件描述符.错误返回-1,可以有参数,但是linux 2.6.8版本以后可以忽略但是值必须是大于0的.

* epoll_ctl:控制epoll 实例描述符的相关操作

    * epoll的描述符

    * 控制类型

        * EPOLL_CTL_ADD:添加fid 进入监控状态

        * EPOLL_CTL_MOD:修改fid 事件装填

        * EPOLL_CTL_DEL:删除fid 取消监控状态

        * [更多参考](http://man7.org/linux/man-pages/man2/epoll_ctl.2.html)

    * 参数3:被监控的fid

    * 参数4:事件数据

    * 添加监控描述符:将fd加入监控描述符中

    ```c
    int epollAdd(int epollfd,int fd, int eventType){
        struct epoll_event e;
        setnonblocking(fd);
        //设置与要处理的事件相关的文件描述符
        e.data.fd=fd;
        //设置要处理的事件类型
        e.events=eventType;
        //注册epoll事件
        epoll_ctl(epollfd,EPOLL_CTL_ADD,fd,&e);
        return SW_OK;
    }
    ```

    * 修改监控事件类型:

    ```c
    int epollEventSet(int epollfd, int fd, int eventType) {
        struct epoll_event ev;
        memset(&ev, 0, sizeof(ev));
        ev.data.fd = fd;
        ev.events = eventType;
        int r = epoll_ctl(epollfd, EPOLL_CTL_MOD, fd, &ev);
        return SW_OK;
    }
    ```

* epoll_wait:等待事件相应,返回响应事件的数量,一般阻塞在循环中,常驻检查事件的情况.当事件返回做相应的程序逻辑.

    nfds=epoll_wait(epollfd,local_events,20,500);


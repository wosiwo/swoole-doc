# 双工管道详解

## 功能描述

双工管道:字面意思也是就是一个管道两边都可以流入东西,我觉得更像高速公路,一边进入另一边出来,绝对不能进入后在返航.管道特性就是一边写入,另一边读取.如果没有及时读取会阻塞管道.

## 创建方法

* socketpair:用来创建一对相互连通的socket.

* 参数:

    * 参数1:说明参数链接类型 本机用AF_UNIX, AF_LOCAL 还有其他协议

    * 参数2:置顶socket类型 SOCK_DGRAM 或者 数据流 SOCK_STREAM

    * 参数3:协议类型 Tcp Udp 等等

    * 参数4:返回的文件描述符数组.

* 具体链接类型,socket类型,协议类型请[参考](http://man7.org/linux/man-pages/man2/socket.2.html)

## 代码实现

```
    /**
     * 创建各个worker子进程的管道
     */
    int createWorkerPipe(int workerNum){
        int i;
        int ret;
        int socks[2];

        //需要再master进程中初始化好worker进程所用的管道，在manager进程初始化master进程取不到数据
        for (i = 0; i < workerNum; i++)
        {
            //在master进程中将所有管道都创建好
            ret = socketpair(AF_UNIX, SOCK_DGRAM, 0, socks);
            //获取用于读取的fd
            workers[i].pipWorkerFd = socks[1];
            workers[i].pipMasterFd = socks[0];
        }
        return 1;
    }
```

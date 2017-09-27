# thread详解

## 功能描述

thread 在[基础知识](../append/F3_基础知识.md)里已经讲解了,线程相关知识.在linux中实现线程的方式利用pthread_create 函数,复杂的多线程管理暂且不管,整体类似php的call_user_function,就是起一个线程来执行某一段代码.

## 参数

* 参数1:pthread_t类型的线程标识符

* 参数2:线程属性设置 null表示采用默认

* 参数3:线程函数的启示地址(方法名)

* 参数4:方法的参数


## 返回值

-1 :标识出错.

=0 :表示成功

## 代码示例
    //批量创建线程
    int reactorNum = 2;
    pthread_t pidt;
    for (i = 0; i < reactorNum; i++)
    {
        //创建线程 执行swReactorThread_loop方法
        if (pthread_create(&pidt, NULL,swReactorThread_loop, i) < 0)
        {
            printf("pthread_create[tcp_reactor] failed. Error: %s[%d]", strerror(errno), errno);
        }
        printf("pthread_create  %d pidt %d \n",i,pidt);
    }


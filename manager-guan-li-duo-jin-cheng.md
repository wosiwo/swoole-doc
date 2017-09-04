reactor多线程和worker多进程的逻辑

主进程创建manager子进程，manager子进程创建多个worker子进程,每个子进程创建一个自己的reactor线程，用于监听管道事件，

manager进程还负责管理worker子进程的后续销毁与重建



主进程循环创建 reactor线程，每个线程监听一个管道事件，reactor线程接受到连接后取出数据，写入管道，等待worker进程监听到管道事件

worker进程接收到pipe 读事件后，从管道取出请求数据，以及连接socket 的fd，对数据类型转换为zval，然后通过call\_\_user\_function，调用事先设置好的php回调函数

php代码中接收到fd,与请求数据\(仍然处于worker进程中\)，按照约定好的协议进行解包，根据请求内容执行相关逻辑，调用send方法发送数据，

send方法实现在扩展的php\_method中，最终将返回数据写入与reactor线程监听的管道中（worker进程接收到reactor进程传过来的连接fd,与线程id,传回给reactor线程时，使用连接fd,与线程id取到reactor线程监听的管道fd）

//TODO worker进程写入的是否是开始那个reactor的管道

reactor线程，监听到管道事件，取出数据，并修改当前连接fd的epoll事件类型为out,当前线程的epoll循环到这个out事件后，将返回数据写入当前连接的fd中

//TODO 是否能直接写入fd,不经过epoll out


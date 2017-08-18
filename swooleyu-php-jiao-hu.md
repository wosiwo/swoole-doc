初始化swoole，设置回调

swoole\_server.c

PHP\_METHOD\(swoole\_server, on\)

服务端发送数据给客户端

PHP\_METHOD\(swoole\_server, send\)

send流程

swoole\_server.send-&gt;swServer\_tcp\_send-\[&gt;factory-&gt;finish\]-&gt;swFactoryProcess\_finish-&gt;swWorker\_send2reactor-&gt;swSocket\_write\_blocking/write pipe


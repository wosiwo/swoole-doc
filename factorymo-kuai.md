```
swServer_create -> swReactorThread_create->swFactoryProcess_create 中设置Factory操作函数
```

进入worker循环

```
swFactoryProcess_start->swManager_start->swManager_loop_sync
```

main\_reactor

```
main_reactor在swServer_start_proxy中定义

swServer_start->swServer_start_proxy->swServer_master_onAccept
```

main\_reactor 进行select 循环监听，接受到连接后触发 listen事件

连接调用流程

```
epoll_wait -> accept -> getHandle(read_event)-> swReactorThread_onRead(读取数据)->swPort_onRead_check_length[port->onRead]
->swProtocol_recv_check_length->swReactorThread_dispatch->[factory->swFactoryProcess_dispatch]->
swReactorThread_send2worker->[从main_reactor抛给reactor线程处理]swWorker_onPipeReceive->  抛给worker进程
```




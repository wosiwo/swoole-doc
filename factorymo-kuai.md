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




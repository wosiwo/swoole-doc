# fork详解

## 功能描述

fork 函数不明思议,分叉.当程序中使用了fork,便在当前点分叉,fork 的次数就是创建子进程的次数,子进程从分叉开始执行.

## 返回值

\>0 :当前是父进程.

=0 :表示当前是子进程.

<0 :表示错误

## 代码示例

    pid_t tyManager_spawn_worker(int worker_id)//fork worker进程
    {
        pid_t pid;
        int ret;
        pid = fork(); //执行fork
        if (pid < 0) //错误判断
        {
            swWarn("Fork Worker failed. Error: %s [%d]", strerror(errno), errno);
            return SW_ERR;
        }
        //子进程做的事
        else if (pid == 0)
        {
    		printf("worker child processor worker_id %d \n",worker_id);
    		//TODO 监听pipe管道事件
    		workers[worker_id].pid = pid;
    		worker = workers[worker_id];
    		printf("worker master %d \n",worker.pipMasterFd);
            ret = tyWorker_loop( worker_id);
            exit(ret);
        }
        //父进程做的事
        else
        {
            printf("worker child pid %d \n",pid);
            return pid;
        }
    }


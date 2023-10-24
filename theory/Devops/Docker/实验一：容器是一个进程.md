# 实验一：容器的本质是一个进程

理解容器的本质：一个启用了多个 Linux Namespace 的**应用进程** 



实验前提：

1.  安装好docker

```shell
# 运行busybox容器中的sh
docker run -it busybox sh

# 运行busybox容器睡3600秒
docker run -d busybox sleep 3600

# 小技巧：批量操作容器
docker stop $(docker ps -aq)

# 查看进程树
docker container top <id>
yum install -y psmisc
pstree -halps <PID>

# 查看容器真实PID
docker inspect --format "{{.State.Pid}}"  <id>
```


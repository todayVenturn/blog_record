# 实验三：Cgroup限制

## 手动设置Cgroup

cfs_period 和 cfs_quota。这两个参数需要组合使用，可以用来限制进程在长度为 cfs_period 的一段时间内，只能被分配到总量为 cfs_quota 的 CPU 时间。

```shell
# 显示Cgroup的挂载目录
mount -t cgroup

# 创建container文件夹
mkdir -p /sys/fs/cgroup/cpu/container

# 无限循环，将单核CPU打满
while : ; do : ; done &

# 查看CPU quota
cat cpu.cfs_quota_us

# 查看CPU period 默认100ms, 每100ms的时间里, 该进程组能使用100ms，相当于100%
cat cpu.cfs_period_us

# 设置Cgroup规则
echo 20000 > /sys/fs/cgroup/cpu/container/cpu.cfs_quota_us

# 添加控制组内的进程
echo <Pid> > /sys/fs/cgroup/cpu/container/tasks 
```



## 查看docker设置的Cgroup

```shell
# 运行ubuntu，指定Cgroup的控制参数
docker run -it --cpu-period=100000 --cpu-quota=20000 ubuntu /bin/bash

# 查看docker容器的控制组参数
ls /sys/fs/cgroup/cpu/docker/
```


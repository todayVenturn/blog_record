# 实验四：僵尸进程

```shell
# 运行僵尸进程演示容器
docker run --name zombie-proc -d registry/zombie-proc:v1
docker exec -it zombie-proc bash


# 让init进程作为容器的1号进程
docker run -it --init --rm ubuntu /bin/bash

# 进程最大值
# Linux 内核在初始化系统的时候，会根据机器 CPU 的数目来设置 pid_max 的值
# CPU 数目小于等于 32， pid_max 就会被设置为32768（32K）
# CPU 数目大于 32，那么 pid_max 就被设置为 N*1204（N 就是 CPU 数目）
cat /proc/sys/kernel/pid_max
```


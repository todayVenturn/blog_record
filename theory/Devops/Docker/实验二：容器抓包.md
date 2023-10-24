# 实验二：容器抓包

​	Linux抓包非常简单，使用tcpdump就可以进行抓包，但是很多时候为了减少镜像大小，会最大限度的将不必要的软件删除，所以很多基础镜像就没有tcpdump命令，如果此时你的机器正好在无网络环境，你无法用yum命令安装tcpdump，这时你想抓包就会非常麻烦。



```shell
# 获取容器真实PID
docker inspect --format "{{.State.Pid}}"  <容器id>

# 这种方式也可以，但是看起来比较乱，要找到容器中的第一个PID，一般是run.sh的那个
docker top <容器id>

# 进入该容器的网络namespace
nsenter -n -t  <Pid>

# 使用ifconfig验证，现在你就在容器的网络环境了，可以看到网卡信息和你在容器内看到的是一样的
ifconfig
```



> 注意点：
>
> - 这种方式不需要用K8S命令，完全使用docker，命令比较简单
> - 但是K8S有Master 节点和Node节点，需要确认一下你的容器被安装到那个节点，就去那个节点获取容器ID，不然是获取不到的
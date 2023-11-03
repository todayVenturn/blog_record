# 实验五：探索容器网络模型

```shell
# 使用host方式运行Nginx
docker run -d --net host nginx

# 宿主机和容器之间复制文件
# 宿主机往容器内拷贝
docker cp ./data <containerId>:/root
# 容器网宿主机拷贝
docker cp <containerId>:/data /root
```





```shell
# dockerfile示例
FROM centos:7
COPY nginx.repo /etc/yum.repos.d/nginx.repo
RUN yum install -y nginx 
EXPOSE 80
ENV HOST=mynginx
CMD ["nginx","-g","daemon off;"]
```


# CentOS 7.9 安装 MySQL 5.7

## 1. 下载安装包

[MySQL官网](https://downloads.mysql.com/archives/community/)

```bash
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.42-1.el7.x86_64.rpm-bundle.tar
```





## 2. 按照顺序安装RPM

卸载自带的`mariadb-libs`

```
rpm -e mariadb-libs-5.5.68-1.el7.x86_64
```

安装RPM

```bash
rpm -ivh mysql-community-common-5.7.42-1.el7.x86_64.rpm

rpm -ivh mysql-community-libs-5.7.42-1.el7.x86_64.rpm  --force --nodeps

rpm -ivh mysql-community-client-5.7.42-1.el7.x86_64.rpm 

rpm -ivh mysql-community-server-5.7.42-1.el7.x86_64.rpm 
```



## 3. 配置MySQL配置

```bash
vim /etc/my.cnf

[mysqld]
# 跳过检测
skip-grant-tables

datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
```



重启MySQL，设置MySQL开机自启动

```bash
systemctl restart mysqld

systemctl enable mysqld
```



## 4. 设置root密码

```bash
mysql

mysql> update mysql.user set authentication_string=password('123456') where user='root';

systemctl restart mysqld
```



## 5. 取消跳过检测

```bash
vim /etc/my.cnf

[mysqld]

datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid


systemctl restart mysqld
```



## 6. 设置允许root远程登录

```bash
set global validate_password_length=4;

set password=password('123456');

grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
```


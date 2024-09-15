Docker - Mysql8.0.32

## 一、Mysql 容器化部署

### 0.目录准备:

```bash
#创建mysql运行目录

[root@SRC105036 mysql]# pwd
/home/chennl/mysql

[root@SRC105036 mysql]# ls
conf  data  log

# conf 为mysql配置目录
[root@SRC105036 mysql]# ls conf
my.cnf

#  data 为mysql数据文件目录
[root@SRC105036 mysql]# ls data
## rm -rf data  #可以删除整个目录，包含目录里面的子目录和文件
## mkdir data


#  log 为mysql数据文件目录
[root@SRC105036 mysql]# ls log

#拉取  mysql镜像
##1.拉取镜像--  latest mysql8.0.32
[root@SRC105036 mysql]# docker pull mysql:8.0.32
```



### 1. 数据库配置文件 

```bash
[root@SRC105036 mysql]# vim conf/my.cnf
```

```ini
[client]
#设置客户端默认字符集
default-character-set=UTF8MB4

[mysqld]
#默认字符集
character-set-server = UTF8MB4

 

####以下配置做为 master-slaver模式需要的配置

###主从数据库配置开始
    #该服务节点的唯一标识
    server-id = 100
    #制定复制的数据库
    binlog-do-db = swiredb

    #开启bin-log,并且设置日志文件名
    log-bin = mysql-bin
    #默认日志格式 row(默认) ,statement,mixed
    binlog-format = statement
    #设置每间隔多少次事务才会将操作写到binlog
    sync-binlog = 1
    #日志超过7天自动清除
    binlog_expire_logs_seconds=604800
    #默认使用"mysql-native-password"
    
###Master-Slaver 配置结束

```

### 2.创建容器

```bash

##2.运行容器
[root@SRC105036 mysql]# docker run -d  --name mysql_master \
-v /home/chennl/mysql/data:/var/lib/mysql \
-v /home/chennl/mysql/conf:/etc/mysql/conf.d \
-v /home/chennl/mysql/logs:/var/log/mysql \
-p3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:8.0.32

8c5f938dd5cb5eafc281314385251a8e4f48868852031fb1030955c7b2895a68

## SHOW VARIABLES like '%log%'   可以查mysql各种变量

##3.查看容器详情
[root@SRC105036 mysql]# docker inspect 8c5f938dd5cb5eaf
#看到挂载目录
        "Mounts": [
            {
                "Type": "bind",
                "Source": "/home/chennl/mysql/data",
                "Destination": "/var/lib/mysql",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "/home/chennl/mysql/conf",
                "Destination": "/etc/mysql/conf.d",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "/home/chennl/mysql/log",
                "Destination": "/var/log/mysql",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],

#看到IP+端口
  "Ports": {
                "3306/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "3316"
                    }
                ],
                "33060/tcp": null
            },
             {
                    "Gateway": "172.17.0.1",  
                    "IPAddress": "172.17.0.19",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:13",
                    "DriverOpts": null
                }
            }

###  查看容器日志
[root@SRC105036 mysql]# docker logs 7b13d7d339aa

### 查找images名字为abc的镜像
docker ps | grep abc

###查找 name为mysql开始的容器
[root@SRC105036 mysql]# docker ps |grep mysql
	6c53522ffe9c   mysql:8.0.32    。。。。。
	
[root@SRC105036 mysql]# docker stop 6c53522ffe9c
	6c53522ffe9c
	
[root@SRC105036 mysql]# docker rm 6c53522ffe9c
	6c53522ffe9c
	
##进入容器查询数据库
[root@SRC105036 mysql]#  docker exec -it 7b13d7d339aa bash
bash-4.4# mysql -uroot -p123456
mysql> CREATE DATABASE `swiredb` CHARACTER SET 'utf8mb4';
mysql> use swiredb
mysql> DROP TABLE IF EXISTS `ci_company`;
mysql> CREATE TABLE `ci_company`  (
  `code` int NOT NULL,
  `name` varchar(255) NULL DEFAULT NULL,
    PRIMARY KEY (`code`) USING BTREE
   ) ;

mysql> INSERT INTO `users` VALUES (3006,'浙江太古可口可乐有限公司 '),(3045,'上海太古可口可乐有限公司')

mysql> show tables;

mysql> select * from ci_company;
+------+--------------------------------------+
| code | name                                 |
+------+--------------------------------------+
| 3006 | 浙江太古可口可乐有限公司             |
| 3045 | 上海太古可口可乐有限公司             |
+------+--------------------------------------+
2 rows in set (0.00 sec)
mysql> exit
Bye
bash-4.4# exit
exit

```



## 二、Master/Slave部署

### 0. 环境准备

```shell
##  主数据库目录: mysql
##  主数据库目录: mysql-slave
[root@SRC105036 chennl]# ls
   mysql  mysql-slave
[root@SRC105036 mysql-slave]# ls
   conf  data  log


```



### 1.主数据库配置文件

参考 一.1部分

### 2. 从数据库配置文件

```shell

```


# Redis

## Nosql 概述

### 为什么使用 Nosql

大数据时代

####  Mysql引擎:

- MyISAM: 表锁
- innoDB: 行锁

#### 数据库本质：读写

- 垂直：读写分离 解决写的问题

  ![image-20221116222733578](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221116222733578.png)

- 水平：分库分表+mysql集群 解决写的问题

![image-20221116222836131](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221116222836131.png)

#### 文件，博客，照片，音乐存储

互联网基本架构：

![image-20221116223826281](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221116223826281.png)

## Nosql

不仅仅是SQL,泛指非关系型数据库

![image-20221116224807494](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221116224807494.png)

## Redis下载安装

![image-20221126132531855](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126132531855.png)

1. 下载安装包: redis-7.0.5.tar.gz

2. 解压安装包: 

- 程序一般放到 /opt下面 

  ``` bash 
  # mv redis-7.0.5.tar.gz /opt
  # tart -vxzf redis-7.0.5.tar.gz
  # ls
   containerd  redis-7.0.5  redis-7.0.5.tar.gz
   
  ```

   ![image-20221126133935847](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126133935847.png)

- 进入解压后文件，可以看到redis配置文件

- 基本环境安装

  ``` bash
  yum install -y gcc-c++
  make
  ```

  ![image-20221126134502937](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126134502937.png)

  ``` bash
  # make之后生成新目录 scr
  cd src
  make install
  ```

  

![image-20221126134634414](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126134634414.png)



4. 默认安装路径： /usr

本地自己安装路径一般是 /usr/local/bin下

![image-20221211104959895](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221211104959895.png)

![image-20221126134956451](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126134956451.png)

5. 配置
   - 将Redis配置文件拷贝到当前目录. 创建一个自己配置文件目录nconfig

![image-20221126135547410](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126135547410.png)
   - 修改配置： 默认不是后台启动，所以，要修改配置文件 

![image-20221126135933362](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126135933362.png)



5. 启动

``` bash
 redis-server nconfig/redis.conf
```

![image-20221126140256379](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126140256379.png)

6. 测试

![image-20221126140607070](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126140607070.png)

7. 查看进程

   ![image-20221126154419716](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126154419716.png)

8. 关闭服务

   ``` bash
   shutdown
   ```
   
   ![image-20221126154619469](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221126154619469.png)

9. 绑定其他IP,供远程连接

   ```bash
   
   # bind 192.168.1.100 10.0.0.1     # listens on two specific IPv4 addresses
   # bind 127.0.0.1 ::1              # listens on loopback IPv4 and IPv6
   # bind * -::*                     # like the default, all available interfaces
   #
   # ~~~ WARNING ~~~ If the computer running Redis is directly exposed to the
   # internet, binding to all the interfaces is dangerous and will expose the
   # instance to everybody on the internet. So by default we uncomment the
   # following bind directive, that will force Redis to listen only on the
   # IPv4 and IPv6 (if available) loopback interface addresses (this means Redis
   # will only be able to accept client connections from the same host that it is
   # running on).
   #
   # IF YOU ARE SURE YOU WANT YOUR INSTANCE TO LISTEN TO ALL THE INTERFACES
   # COMMENT OUT THE FOLLOWING LINE.
   #
   # You will also need to set a password unless you explicitly disable protected
   # mode.
   # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   #第1步：绑定IP
   bind 127.0.0.1 -::1 192.168.249.103
   #第2步：去掉保护模式
   protected-mode no
   #或者设置密码(eg:设置密码为: root)
   
   ```

   

10. 开端口

    ```bash
    #查看端口
    [root@localhost bin]# firewall-cmd --list-ports
    8080/tcp
    #6379没开，开启这个端口 
    [root@localhost bin]# firewall-cmd --add-port=6379/tcp --permanent
    success
    #刷新网络
    [root@localhost bin]# firewall-cmd --reload
    success
    #查看端口
    [root@localhost bin]# firewall-cmd --list-ports
    8080/tcp 6379/tcp
    
    ```

    

##  性能测试

官方自带性能测试。Redis 性能测试是通过同时执行多个命令实现的。

### 语法

redis 性能测试的基本命令如下：

```
redis-benchmark [option] [option value]
```

| 序号 | 选项                      | 描述                                       | 默认值    |
| :--- | :------------------------ | :----------------------------------------- | :-------- |
| 1    | **-h**                    | 指定服务器主机名                           | 127.0.0.1 |
| 2    | **-p**                    | 指定服务器端口                             | 6379      |
| 3    | **-s**                    | 指定服务器 socket                          |           |
| 4    | **-c**                    | 指定并发连接数                             | 50        |
| 5    | **-n**                    | 指定请求数                                 | 10000     |
| 6    | **-d**                    | 以字节的形式指定 SET/GET 值的数据大小      | 2         |
| 7    | **-k**                    | 1=keep alive 0=reconnect                   | 1         |
| 8    | **-r**                    | SET/GET/INCR 使用随机 key, SADD 使用随机值 |           |
| 9    | **-P**                    | 通过管道传输 <numreq> 请求                 | 1         |
| 10   | **-q**                    | 强制退出 redis。仅显示 query/sec 值        |           |
| 11   | **--csv**                 | 以 CSV 格式输出                            |           |
| 12   | ***-l\*（L 的小写字母）** | 生成循环，永久执行测试                     |           |
| 13   | **-t**                    | 仅运行以逗号分隔的测试命令列表。           |           |
| 14   | ***-I\*（i 的大写字母）** | Idle 模式。仅打开 N 个 idle 连接并等待。   |           |

### 实例

``` bash
#测试 100并发，10000个请求
redis-benchmark -c 50 -n 1000
```

![image-20221127150359366](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221127150359366.png)![image-20221127150424490](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221127150424490.png)



## Redis基本知识

1. 默认 16个数据库

![image-20221127150701152](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221127150701152.png)

2. 使用select切换数据库

   ```bash
   select 3
   dbsize
   [root@localhost bin]# redis-cli 
   127.0.0.1:6379> select 3   #选择3号数据库
   OK
   127.0.0.1:6379[3]> dbsize  #查看数据库大小
   (integer) 0
   127.0.0.1:6379[3]> set name 'abc'  #设置健值
   OK
   127.0.0.1:6379[3]> dbsize
   (integer) 1
   127.0.0.1:6379[3]> keys * #查看所有key值
   127.0.0.1:6379[3]>  flushdb  #清除数据库
   OK
   127.0.0.1:6379[3]> dbsize
   (integer) 0
   127.0.0.1:6379[3]> keys *
   (empty array)
   127.0.0.1:6379[3]> flushall #清除所有数据量数据
   OK
   
   ```

3. redis 单线程

   CPU不是瓶颈，单线程。 C++写的

   ![image-20221127151937643](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221127151937643.png)

##  五大数据类型

### redis-key



``` bash
127.0.0.1:6379[3]> set name chennl
OK
127.0.0.1:6379[3]> exists name #查看name是否存在
(integer) 1
127.0.0.1:6379[3]> set age 17
OK
127.0.0.1:6379[3]> move name 1    #在第1个数据库中将name删除
(integer) 1
127.0.0.1:6379[3]> keys *
1) "age"
127.0.0.1:6379[3]> expire name 10  #设置 name 10秒过期
(integer) 0
127.0.0.1:6379[3]> ttl name  # 查看还有几秒过期
(integer) -2		# -2代表过期了
127.0.0.1:6379[3]> keys *
1) "age"
127.0.0.1:6379[3]> clear #清屏
127.0.0.1:6379[3]> get name
(nil) #表示没有这个key
127.0.0.1:6379[3]> get name
(nil)
127.0.0.1:6379[3]> set name chennl
OK
127.0.0.1:6379[3]> set age 17
OK
127.0.0.1:6379[3]> type name  #查询key类型
string
127.0.0.1:6379[3]> type age
string

```

### String类型

```bash
127.0.0.1:6379[3]> append name ",Hello!"   #如果key不存在，则会增加key
(integer) 13
127.0.0.1:6379[3]> get name
"chennl,Hello!"
127.0.0.1:6379[3]> set view 0
OK
127.0.0.1:6379[3]> incr view
(integer) 1
127.0.0.1:6379[3]> type view
string
127.0.0.1:6379[3]> decr view
0
127.0.0.1:6379[3]> decr view
(integer) 0
127.0.0.1:6379[3]> incrby view 10
(integer) 10
127.0.0.1:6379[3]> decrby view 5
(integer) 5
127.0.0.1:6379[3]> getrange name 1 5
"hennl"

127.0.0.1:6379[3]> getrange name 1 5  #截取字符
"hennl"
127.0.0.1:6379[3]> setrange name 3 "AA"    #替换字符
(integer) 13
127.0.0.1:6379[3]> get name
"cheAAl,Hello!"
127.0.0.1:6379[3]> set name abc
OK
127.0.0.1:6379[3]> get name
"abc"
127.0.0.1:6379[3]> setex name 5 ABC   #5秒过期，过期后设置为 "ABC"
OK
127.0.0.1:6379[3]> get name
"ABC"
127.0.0.1:6379[3]> exists job  
(integer) 0
127.0.0.1:6379[3]> setnx job fire  #如果不存在，则创建key
(integer) 1
127.0.0.1:6379[3]> get job
"fire"
127.0.0.1:6379[3]> mset k1 v1 k2 v2  k3 v3 k4 v4  #批量创建
OK
127.0.0.1:6379[3]> keys *
1) "k3"
2) "k1"
3) "k4"
4) "k2"
127.0.0.1:6379[3]> mget k1 k2 k3 k4
1) "v1"
2) "v2"
3) "v3"
4) "v4"
# 对象
#巧妙处理json
127.0.0.1:6379[3]> set user:1 {name:cnl,age:17,grade:6}    
OK
127.0.0.1:6379[3]> get user:1
"{name:cnl,age:17,grade:6}"
#getset

127.0.0.1:6379[3]>  getset  user:1 {name:chy,age:17,grade:6}
"{name:cnl,age:17,grade:6}"


```

![image-20221127185859461](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221127185859461.png)

## List

```bash
127.0.0.1:6379[3]> lpush list 1   #将1个或多个值 放入列表头部
(integer) 1
127.0.0.1:6379[3]> lpush list 2 3 4 5
(integer) 5
127.0.0.1:6379[3]> lrange list 0 -1
1) "5"
2) "4"
3) "3"
4) "2"
5) "1"
```



## Set

集合，但不能重复

``` bash
127.0.0.1:6379[3]> sadd myset "hello"
(integer) 1
127.0.0.1:6379[3]> sadd myset kuangshen
(integer) 1
127.0.0.1:6379[3]> smembers myset
1) "kuangshen"
2) "hello"
127.0.0.1:6379[3]> sismember myset hello
(integer) 1


```

## Redis 哈希(Hash)

Redis hash 是一个 string 类型的 field（字段） 和 value（值） 的映射表，hash 特别适合用于存储对象。

```
127.0.0.1:6379>  HMSET runoobkey name "redis tutorial" description "redis basic commands for caching" likes 20 visitors 23000
OK
127.0.0.1:6379>  HGETALL runoobkey
1) "name"
2) "redis tutorial"
3) "description"
4) "redis basic commands for caching"
5) "likes"
6) "20"
7) "visitors"
8) "23000"
```

## Redis GEO

Redis GEO 主要用于存储地理位置信息，并对存储的信息进行操作，该功能在 Redis 3.2 版本新增。

Redis GEO 操作方法有：

- geoadd：添加地理位置的坐标。

- geopos：获取地理位置的坐标。

- geodist：计算两个位置之间的距离。

- georadius：根据用户给定的经纬度坐标来获取指定范围内的地理位置集合。

- georadiusbymember：根据储存在位置集合里面的某个地点获取指定范围内的地理位置集合。

- geohash：返回一个或多个位置对象的 geohash 值。

  ``` bash
  ```

  

## Redis集群环境搭建

### 一主二从

1. 复制3个配置文件并修改端口分别为6379,6380,6381

   ``` bash
   root@localhost nconfig]# cp redis.conf redis80.conf
   [root@localhost nconfig]# cp redis.conf redis81.conf
   [root@localhost nconfig]# ls
   redis80.conf  redis81.conf  redis.conf
   
   ```

2. 修改配置文件

   ![image-20221128202555774](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128202555774.png)

   ![image-20221128202645161](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128202645161.png)

   ![image-20221128202727878](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128202727878.png)

   ![image-20221128202815841](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128202815841.png)

3. 启动三台Redis

   默认每一台都是主机

   ![image-20221128203457662](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128203457662.png)



4. 配置从机（79 主机，80，81从机)

   > 通过命令配置

   ```bash
   127.0.0.1:6379> info replication
   # Replication
   role:master
   connected_slaves:0
   master_failover_state:no-failover
   master_replid:9425cab65f5b3d8cc5b98b393fffd390d73331d0
   master_replid2:0000000000000000000000000000000000000000
   master_repl_offset:0
   second_repl_offset:-1
   repl_backlog_active:0
   repl_backlog_size:1048576
   repl_backlog_first_byte_offset:0
   repl_backlog_histlen:0
   
   ```

   ![image-20221128203812578](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128203812578.png)

``` bash
127.0.0.1:6380> slaveof 127.0.0.1 6379
OK
127.0.0.1:6380> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6379
master_link_status:up
master_last_io_seconds_ago:2
master_sync_in_progress:0
slave_read_repl_offset:14
slave_repl_offset:14
slave_priority:100
slave_read_only:1
replica_announced:1
connected_slaves:0
master_failover_state:no-failover
master_replid:a4affd28bacbedc540ef7334aca55dc92f94063d
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:14
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:15
repl_backlog_histlen:0

```

![image-20221128204054939](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128204054939.png)

![image-20221128204142292](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128204142292.png)

![image-20221128204343509](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128204343509.png)

![image-20221128204425823](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128204425823.png)

> 通过配置文件配置

![image-20221128205107349](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128205107349.png)

5. 主机能写，从机只读

    

```bash 
127.0.0.1:6380> set age 15
(error) READONLY You can't write against a read only replica.
127.0.0.1:6381> set age 15
(error) READONLY You can't write against a read only replica.
127.0.0.1:6379> set age 17  #主机能写
OK
127.0.0.1:6380> get age  #从机能读
"17"
127.0.0.1:6381> get age  #从机能读
"17"
```

![image-20221128210353060](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128210353060.png)

> 哨兵模式

![image-20221128214946219](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128214946219.png)

![image-20221128215056795](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128215056795.png)

> 哨兵配置文件 sentinel.conf

``` bash
#配置文件
vim sentinel.conf
sentinel monitor myredis 127.0.0.1 6379 1
#启动哨兵
redis-sentinel nconfig/sentinel.conf
```

![image-20221128220102425](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128220102425.png)

```bash
29584:X 27 Nov 2022 07:20:01.294 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
29584:X 27 Nov 2022 07:20:01.301 * Sentinel new configuration saved on disk
29584:X 27 Nov 2022 07:20:01.301 # Sentinel ID is 84ed8e538d55eb4d7cc822ee343e93477d02bf09
29584:X 27 Nov 2022 07:20:01.301 # +monitor master myredis 127.0.0.1 6379 quorum 1
29584:X 27 Nov 2022 07:20:01.303 * +slave slave 127.0.0.1:6380 127.0.0.1 6380 @ myredis 127.0.0.1 6379
29584:X 27 Nov 2022 07:20:01.309 * Sentinel new configuration saved on disk
29584:X 27 Nov 2022 07:20:01.309 * +slave slave 127.0.0.1:6381 127.0.0.1 6381 @ myredis 127.0.0.1 6379
29584:X 27 Nov 2022 07:20:01.314 * Sentinel new configuration saved on disk

```

此时关闭6379. 则哨兵自动选举6380为master:

![image-20221128220558120](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128220558120.png)

![image-20221128220744066](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128220744066.png)

## 缓存穿透与雪崩

# 1. 概述

缓存穿透和缓存雪崩是在实际项目中，经常能遇到的问题。

今天我们就简单聊聊缓存穿透和缓存雪崩的这两个话题。

# 2. 缓存穿透

## 2.1 什么是缓存穿透？

简单说就是用户发起请求时，始终匹配不到缓存中的数据，每次都直接通过关系型数据库进行查询，并得到数据。

如果这个请求的并发量非常的大，非常多的用户在同一时刻去执行这个请求，那么会超出关系型数据库的负载，从而导致数据库的宕机。

 ![image-20221128221415373](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128221415373.png)

![image-20221128221841515](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128221841515.png)

## 2.2 解决方案一：优化代码逻辑

其中一个解决方案，就是编写代码时，逻辑要严谨，反复自测，保证任何条件的查询，都一定是先经过缓存进行查询。

如果缓存中没有查到数据，则进行一次关系型数据库的查询，然后将查询结果存储到缓存中（即使查询结果是空值，也将空值存储到缓存中），保证下一次查询可以从缓存中获取。

当然，由于数据库中的数据会随时变化，缓存是需要有过期时间的。

 2.3 解决方案二：布隆过滤器

### 2.3.1 布隆过滤器简介

布隆过滤器能够很快的判断某个元素是否已存在集合中。

布隆过滤器占用内存小，读写非常快。

布隆过滤器适合于缓存中存在过某key才去查询缓存，没存在过，就不去查询直接返回空的场景。

布隆过滤器通常放在缓存前面执行，可以将缓存的key放进布隆过滤器中，读取数据时，如果布隆过滤器判断缓存的key存在，才会到缓存中去查询，不存在就直接返回空，大大降低了缓存穿透的可能。

 3.缓存雪崩

## 3.1 什么是缓存雪崩？

由于数据库中的数据是随时变化的，因此缓存中的key通常都是有过期时间的。

在某一时刻，缓存中的大面积的key都失效了，恰好此时，很多请求并发到来，直接访问了关系型数据库，从而导致数据库宕机，这就是缓存雪崩。

## 3.2 如何预防缓存雪崩

1）缓存永不过期

缓存不设置过期时间，而是使用程序手动让其过期。

2）错开缓存的过期时间

在设置缓存过期时间时，加一个固定范围的随机数，从而错开缓存过期时间。



 ![image-20221128222055803](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221128222055803.png)

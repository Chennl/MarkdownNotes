# MySQL8

## 1. Installation

### - Install repository

  ~~~perl
  # 下载rpm
  mysql84-community-release-el9-1.noarch.rpm
  # Adding the MySQL Yum Repository
  [root@nodeone local]# yum localinstall   mysql84-community-release-el9-1.noarch.rp
  Installed:
    mysql84-community-release-el9-1.noarch
  Complete!
  
  # 查看安装
  [root@nodeone local]# yum repolist  all |grep mysql
  [root@nodeone local]# yum repolist enabled | grep mysql.*-community
  mysql-8.4-lts-community               MySQL 8.4 LTS Community Server
  mysql-connectors-community            MySQL Connectors Community
  mysql-tools-8.4-lts-community         MySQL Tools 8.4 LTS Community
  
  
  ~~~

### - Installing MySQL

  ~~~sh
  [root@localhost var]# yum install mysql-community-server -y
  ~~~

### - Starting the MySQL Server

  ```sh
  # 1. Start mysqld
   [root@localhost var]# systemctl start mysqld
   [root@localhost var]# systemctl status mysqld
  
  # 2. Supperuser account 'root'@'localhost' is created. A password for the supper user is set and stored in error log file.
  # To reveal it, use the following command:
   [root@nodeone local]# grep 'temporary password' /var/log/mysqld.log
  2024-09-04T16:04:47.933980Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: <jW/v+!j5#Wl
  
  
  ```

### - **Change password**

  ```sh
  # Change the root password as soon as possible by logging in with the generated, temporaty password and set a customer password for the supperuser account:
  [root@nodeone local]# mysql -uroot -p
  # Update validate password to low level
  
  mysql> show variables like '%validate_password%';
  +-------------------------------------------------+--------+
  | Variable_name                                   | Value  |
  +-------------------------------------------------+--------+
  | validate_password.changed_characters_percentage | 0      |
  | validate_password.check_user_name               | ON     |
  | validate_password.dictionary_file               |        |
  | validate_password.length                        | 8      |
  | validate_password.mixed_case_count              | 1      |
  | validate_password.number_count                  | 1      |
  | validate_password.policy                        | MEDIUM |
  | validate_password.special_char_count            | 1      |
  +-------------------------------------------------+--------+
  8 rows in set (0.00 sec)
  
  
  mysql> set global validate_password.length=4;
  Query OK, 0 rows affected (0.00 sec)
  
  mysql> set global validate_password.policy=LOW;
  Query OK, 0 rows affected (0.00 sec)
  
  mysql> set global validate_password.mixed_case_count =0;
  Query OK, 0 rows affected (0.00 sec)
  
  mysql> alter user 'root'@'localhost' identified by '1234';
  Query OK, 0 rows affected (0.00 sec)
  
  
  ##永久修改安全策略  修改 /etc/my.cnf
  [root@nodeone local]# vim /etc/my.cnf
  
  
  [mysqld]
  
  datadir=/var/lib/mysql
  socket=/var/lib/mysql/mysql.sock
  
  log-error=/var/log/mysqld.log
  pid-file=/var/run/mysqld/mysqld.pid
  
  # 安全策略
  validate_password.policy=LOW
  validate_password.length=4
  validate_password.mixed_case_count=0
  
  
  ```

### - Create Database & Tables

```mysql
mysql> CREATE DATABASE 'bmp' CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;
mysql> CREATE TABLE pet (NAME VARCHAR(20), OWNER VARCHAR(20), species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);
mysql> DESCRIBE pet;
mysql> DESC pet;

```

### - Create user and grants

``` mysql
mysql> CREATE USER 'sa'@'%' IDENTIFIED BY '1234';
mysql> GRANT ALL ON bmp.* TO 'sa'@'%';
```

### - Loading Data into a Table

```mysql
mysql> LOAD DATA LOCAL INFILE '/var/local/pet.txt' TO TABLE pet LINES TERMINATED BY '\r\n';
mysql> INSERT INTO pet VALUES ('Puffball','Diane','hamster','f','1999-03-30',NULL);
mysql> SELECT * FROM pet;
mysql> INSERT INTO pet VALUES ('Fluffy','Harold','cat','f','1993-02-04',NULL),	
('Claws','Gwen','cat','m','1994-03-17',NULL),	
('Buffy','Harold','dog','f','1989-05-13',NULL),	
('Fang','Benny','dog','m','1990-08-27',NULL),	
('Bowser','Diane','dog','m','1979-08-31','1995-07-29'),
('Chirpy','Gwen','bird','f','1998-09-11',NULL),	
('Whistler','Gwen','bird','m','1997-12-09',NULL),
('Slim','Benny','snake','m','1996-04-29',NULL);
```

### - Table file

```sh
[root@nodeone ~]# cd /var/lib/mysql
[root@nodeone bmp]# ls
pet.ibd
```

### - Vmware Networks

- ** Setting for firewall of Vmware**

  ```shell
  # 1. Check firewallD status
  [root@nodeone ~]# systemctl status firewalld
  [root@nodeone ~]# systemctl status firewalld
  ● firewalld.service - firewalld - dynamic firewall daemon
       Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; preset: enabled)
       Active: active (running) since Thu 2024-09-05 03:51:03 CST; 9min ago
  
  # 2. check 3306 port
  [root@nodeone ~]# firewall-cmd --zone=public --query-port=3306/tcp
  no
  #no，表示端口关闭，需要打开3306端口
  # 3. add 3306 port
  [root@nodeone ~]# firewall-cmd --zone=public --add-port=3306/tcp --permanent
  success
  # 4. reload firewall
  [root@nodeone ~]# firewall-cmd --reload
  success
  
  #5. check 3306 port
  [root@nodeone ~]# firewall-cmd --query-port=3306/tcp
  yes
  
  #可以使用mysql 客户端连接了
  


### - Post Installation Setup and Testing

Initializing the Data Directory

```sh
#若需要重新安装数据目录
#1. 删除数据目录  /var/lib/mysql/ 
[root@localhost ~]# ls  /var/lib/mysql/
 
#2.初始化目录
[root@localhost ~]# mysqld --initialize
#3. 通过日志 查初始化密码
[root@localhost ~]# cat /var/log/mysqld.log |grep password
2024-09-01T07:05:40.130429Z 6 [Note] [MY-010454] [Server] A temporary password is 	generated for root@localhost: 1guu=nf.pNrn


#修改密码：
mysql> flush priviledge;
mysql> show variables like 'validate_password%';
+-------------------------------------------------+--------+
| Variable_name                                   | Value  |
+-------------------------------------------------+--------+
| validate_password.changed_characters_percentage | 0      |
| validate_password.check_user_name               | ON     |
| validate_password.dictionary_file               |        |
| validate_password.length                        | 8      |
| validate_password.mixed_case_count              | 1      |
| validate_password.number_count                  | 1      |
| validate_password.policy                        | MEDIUM |
| validate_password.special_char_count            | 1      |
+-------------------------------------------------+--------+
8 rows in set (0.01 sec)

mysql> set global validate_password.policy=LOW;;
Query OK, 0 rows affected (0.01 sec)

mysql> set global validate_password.length =4;
Query OK, 0 rows affected (0.00 sec)

mysql> alter user 'root'@'localhost' identified by '123456';
Query OK, 0 rows affected (0.00 sec)


```



 - 通过etc MySQL查配置文件,查看安装目录

   ```sh
   [root@localhost etc]# ls my*
   my.cnf
   my.cnf.d:
   [root@localhost etc]# cat my.cnf
   # For advice on how to change settings please see
   # http://dev.mysql.com/doc/refman/8.4/en/server-configuration-defaults.html
   
   [mysqld]
   #
   # Remove leading # and set to the amount of RAM for the most important data
   # cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
   # innodb_buffer_pool_size = 128M
   #
   # Remove the leading "# " to disable binary logging
   # Binary logging captures changes between backups and is enabled by
   # default. It's default setting is log_bin=binlog
   # disable_log_bin
   #
   # Remove leading # to set options mainly useful for reporting servers.
   # The server defaults are faster for transactions and fast SELECTs.
   # Adjust sizes as needed, experiment to find the optimal values.
   # join_buffer_size = 128M
   # sort_buffer_size = 2M
   # read_rnd_buffer_size = 2M
   
   datadir=/var/lib/mysql
   socket=/var/lib/mysql/mysql.sock
   
   log-error=/var/log/mysqld.log
   pid-file=/var/run/mysqld/mysqld.pid
   
   ```

    

 - **日志文件**

   **log-error=/var/log/mysqld.log**

   **查初始化密码**：

   ```sh
   [root@localhost ~]# cat /var/log/mysqld.log |grep password
   2024-09-01T07:05:40.130429Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 1guu=nf.pNrn
   ```
   
- **配置文件**

  ```sh
  [root@localhost ~]# vim /etc/my.cnf
  # 在[mysqld]下添加 skip-grant-tables，重启数据库。最后修改密
  # 
  [root@localhost ~]# systemctl restart mysqld
  #修改
  
  ```

  ## 2.表空间
  
  ### 1.逻辑结构
  
  ![image-20240905200739428](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20240905200739428.png)
  
  ## 

## 2.数据存储

### 2.1、存储引擎

默认是innoDB

```sql
SHOW ENGINES;

```

## 3.索引

### 3.1.定义

“索引（index）”是一种数据结构，是帮助MySql高效地获取数据的数据结果，并且它是有序的。 除了数据本身之外，数据库系统还会维护满足特定查找算法的数据结构，这些数据结构使用某种方式引用或指向实际的数据，这样就可以在这些数据结构上实现高级查询算法，这的数据结构就是叫“索引”。 

B+树

### 3.2 索引分类

| 分类     | 含义                                             | 特点                     | 关键字   |
| -------- | ------------------------------------------------ | ------------------------ | -------- |
| 主键索引 | 针对主键创建的索引                               | 默认自动创建，只能是一个 | PRMARY   |
| 唯一索引 | 避免同一张表中的某数据列中的值重复               | 可以多个                 | UNIQUE   |
| 常规索引 | 快速定位特定的值                                 | 可以多个                 |          |
| 全文索引 | 全文索引的是文本中的关键词，而不是比较索引中的值 | 可以多个                 | FULLTEXT |
|          |                                                  |                          |          |

在InnoDB存储引擎中，根据索引的存储，分2类：

| 分类                      | 含义                                                       | 特点                |      |
| ------------------------- | ---------------------------------------------------------- | ------------------- | ---- |
| 聚集索引(Clustered Index) | 将数据存储与索引放到了一块，索引结构的叶子节点保存了行数据 | 必须有,而且只有一个 |      |
| 二级索引(Secondary Index) | 将数据与索引分开存储，索引结构的叶子节点关联的是对应的主键 | 可以存在多个        |      |

聚集索引选取规则:

- 如果存在主键，主键索引就是聚集索引
- 如果不存在主键，将使用第一个唯一(UNIQUE)索引作为聚集索引。
- 如果表没有主键，或没有合适的唯一索引，则innoDB会自动生成一个rowid作为隐藏的聚集索引。

![WXWorkCapture_17257825773276](mysql\WXWorkCapture_17257825773276.png)

![image-20240908160525597](mysql\image-20240908160525597.png)

回表查询: 先到二级索引中查找数据，找到主键值，然后再到聚集索引中根据主键值，获取数据的方式，就称之为回表查询.



#### 3.3 索引操作

创建： create [UNIQUE|PRIMARY] index on table 

```mysql
1.创建索引
CREATE [UNIQUE | FULLTEXT]  INDEX index name ON table name ( index col name,... ) ;
2.查看索引
SHOW INDEX FROM table name ;
3.删除索引
DROP INDEX index name ON table name :
```

##### 3.4 SQL性能分析
- **SQL执行频率**
  MySQL 客户端连接成功后，通过 **show [sessionl|global] status** 命今可以提供服务器状态信息，通过如下指令，可以查看当前数据库的
  INSERT、UPDATE、DELETE、SELECT的访问频次:

``` mysql
SHOW [GLOBAL | SESSION] STATUS [LIKE 'pattern' | WHERE expr]
    
show session status like 'Com_______'
show global status like 'Com_______'
```

通过上述指令，我们可以查看到当前数据库到底是以查询为主，还是以增删改为主，从而为数据库优化提供参考依据。如果是以增
删改为主，我们可以考虑不对其进行索引的优化。如果是以查询为主，那么就要考虑对数据库的索引进行优化了

- **慢查询日志**
  慢查询日志记录了所有执行时间超过指定参数 (long query time，单位: 秒，默认10秒)的所有SQL语句的日志
  MySQL的慢查询日志默认没有开启，我们可以查看一下系统变量 slow query log

  ```mysql 
  SHOW VARIABLES LIKE 'SLOW_QUERY_LOG';
  |Variable_name |Value|
  |--------------|-----|
  |slow_query_log|ON   |
  
  ```

  

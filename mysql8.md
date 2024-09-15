# Mysql

## 一、Mysql安装

### 1.1 Windows安装

1. 官网下载：mysql8.0.31  https://dev.mysql.com/downloads/mysql/

2. 解压: C:\workspace\mysql8031

3. 配置： my.ini

   创建在mysql8031目录（与bin等目录齐驱）下创建my.ini文件，内容如下：

``` ini
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=C:\workspace\mysql8031 
# 设置mysql数据库的数据的存放目录
datadir=C:\workspace\mysql8031\data  
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8MB4
character-set-server=UTF8MB4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 8.0之后默认使用多种认证
authentication_policy=*
default-time_zone='+8:00'
[mysql]
# 设置mysql客户端默认字符集 UTF8MB4
default-character-set=UTF8MB4


```

4. 设置环境变量:

   MYSQL_HOME   C:\workspace\mysql8031

   PATH 追加:   %MYSQL_HOME%\bin

5. 初始化数据库

   ``` cmd
   C:\workspace\mysql8031\bin>mysqld --initialize --console
   2022-12-14T13:38:32.998527Z 0 [System] [MY-013169] [Server] C:\workspace\mysql8031\bin\mysqld.exe (mysqld 8.0.31) initializing of server in progress as process 16712
   2022-12-14T13:38:33.043899Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
   2022-12-14T13:38:34.336293Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
   2022-12-14T13:38:36.005410Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: xu9l.bY_s;jv
   
   ```

   - 生成Data目录。 如果需要重新创建，只要删除data目录，重新执行初始化

   - 查看root 密码： ==xu9l.bY_s;jv==

     如果没有 --console参数，可以到data目录下 查看 .err日志文件

6. 安装mysql服务

   ```cmd
   C:\workspace\mysql8031\bin>mysqld install
   Service successfully installed.
   ```

   删除服务 ：

   ```cmd
   C:\workspace\mysql8031\bin>mysqld --remove mysql
   Service successfully removed.
   ```

7. 启动mysql服务

   ```cmd
   C:\workspace\mysql8031\bin>net start mysql
   MySQL 服务正在启动 .
   MySQL 服务已经启动成功。
   ```

   停止服务：

   ```cmd
   C:\workspace\mysql8031\bin>net stop mysql
   MySQL 服务正在停止.
   MySQL 服务已成功停止。
   ```

8. 进入mysql

   ```cmd
   C:\workspace\mysql8031\bin>mysql -u root -p
   Enter password: ************     (初始化随机密码：xu9l.bY_s;jv )
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 9
   Server version: 8.0.31
   
   Copyright (c) 2000, 2022, Oracle and/or its affiliates.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   ```

   

9. 修改root用户密码为'root':

   ```cmd
   alter user 'root'@'localhost' identified by 'root'
   ```

   密码策略：

   ```bash
   mysql> show variables like 'validate_password%';
   +--------------------------------------+--------+
   | Variable_name                        | Value  |
   +--------------------------------------+--------+
   | validate_password.check_user_name    | ON     |
   | validate_password.dictionary_file    |        |
   | validate_password.length             | 8      |
   | validate_password.mixed_case_count   | 1      |
   | validate_password.number_count       | 1      |
   | validate_password.policy             | MEDIUM |
   | validate_password.special_char_count | 1      |
   +--------------------------------------+--------+
   mysql> set global validate_password.check_user_name =OFF;
   Query OK, 0 rows affected (0.00 sec)
   mysql> set global validate_password.length=4;
   Query OK, 0 rows affected (0.00 sec)
   mysql> set global validate_password.policy=LOW;
   Query OK, 0 rows affected (0.00 sec)
   mysql>  show variables like 'validate_password%';
   +--------------------------------------+-------+
   | Variable_name                        | Value |
   +--------------------------------------+-------+
   | validate_password.check_user_name    | OFF   | # 不检查用户名，只检查长度
   | validate_password.dictionary_file    |       |
   | validate_password.length             | 4     | # 长度最小4
   | validate_password.mixed_case_count   | 1     |
   | validate_password.number_count       | 1     |
   | validate_password.policy             | LOW   |
   | validate_password.special_char_count | 1     |
   +--------------------------------------+-------+
   7 rows in set (0.00 sec)
   
   ```

   

10. dddd

### 1.2 Linux安装Mysql

> **1. 检查是否安装**

``` bash
#检查是否安装
rpm -qa|grep mysql
#检查是否有mysql文件
whereis mysql
#查找mysql相关文件
find / -name mysql
#CentOS7自带mariadb，与MySQL数据库冲突
rpm -qa|grep mariadb
```

![image-20221216211920347](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221216211920347.png)

> **2.卸载已经安装的mysql**

```bash
# rpm -e --nodeps 软件名
rpm -e --nodeps mariadb-libs-5.5.68-1.el7.x86_64
```

> **3.检查mysql用户组、用户是否存在** 

```bash
[root@localhost /]# cat /etc/group | grep mysql
[root@localhost /]# cat /etc/passwd |grep mysql

```

**这里你可以会遇到centos7默认没有wget命令,运行一下命令安装就可以了！**

```bash
yum -y install wget
```

> **4.下载** 

mysql-8.0.31-1.el7.x86_64.rpm-bundle.tar

```bash
mkdir -p /usr/local/mysql
#解压到: /usr/local/mysql
tar -xvf mysql-8.0.31-1.el7.x86_64.rpm-bundle.tar -C  /usr/local/mysql
```

> **5.安装**

按照下面的顺序安装rpm软件包，否则可能会安装失败

```bash
rpm -ivh mysql-community-common-8.0.31-1.el7.x86_64.rpm 
rpm -ivh mysql-community-client-plugins-8.0.31-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-8.0.31-1.el7.x86_64.rpm
rpm -ivh mysql-community-devel-8.0.31-1.el7.x86_64.rpm    --nodeps --force
rpm -ivh mysql-community-libs-compat-8.0.31-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-8.0.31-1.el7.x86_64.rpm
yum install -y net-tools
rpm -ivh mysql-community-icu-data-files-8.0.31-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-8.0.31-1.el7.x86_64.rpm
```
安装后相关目录：
https://dev.mysql.com/doc/refman/8.0/en/linux-installation-rpm.html

| Files or Resources                                           | Location                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Client programs and scripts                                  | `/usr/bin`                                                   |
| [**mysqld**](https://dev.mysql.com/doc/refman/8.0/en/mysqld.html) server | `/usr/sbin`                                                  |
| Configuration file                                           | `/etc/my.cnf`                                                |
| Data directory                                               | `/var/lib/mysql`                                             |
| Error log file                                               | For RHEL, Oracle Linux, CentOS or Fedora platforms: `/var/log/mysqld.log`For SLES: `/var/log/mysql/mysqld.log` |
| log_bin_basename                                             | 目录:   /var/lib/mysql/    文件名： mysql-bin   如 mysql-bin.00001,mysql-bin.00002 |




> **6.启动Mysql**

```bash
[root@localhost mysql]# ^C
[root@localhost mysql]# clear
[root@localhost mysql]# systemctl status mysqld
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
[root@localhost mysql]# systemctl start  mysqld
[root@localhost mysql]# systemctl status mysqld
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2022-12-16 22:50:00 CST; 3s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 8586 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 8668 (mysqld)
   Status: "Server is operational"
    Tasks: 39
   Memory: 443.2M
   CGroup: /system.slice/mysqld.service
           └─8668 /usr/sbin/mysqld

Dec 16 22:49:52 localhost systemd[1]: Starting MySQL Server...
Dec 16 22:50:00 localhost systemd[1]: Started MySQL Server.
[root@localhost mysql]# 

```

```bash
#说明：可以设置开机时启动mysql服务，避免每次开机启动mysql
systemctl enable mysqld     #开机自启动MySQL服务
netstat -tunlp   #查看已启动的服务
netstat -tunlp | grep mysql 
ps -ef | grep mysql     #查看mysql进程
```

### 1.3.**登录MySQL数据库，查阅临时密码**

```bash
cat /var/log/mysqld.log
[root@localhost mysql]# cat /var/log/mysqld.log
2022-12-16T14:49:53.634234Z 0 [System] [MY-013169] [Server] /usr/sbin/mysqld (mysqld 8.0.31) initializing of server in progress as process 8618
2022-12-16T14:49:53.651482Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2022-12-16T14:49:54.704636Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2022-12-16T14:49:55.960005Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: KaQMVogk9f)(
2022-12-16T14:49:59.285130Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.31) starting as process 8668
2022-12-16T14:49:59.300286Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2022-12-16T14:49:59.814848Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2022-12-16T14:50:00.210456Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2022-12-16T14:50:00.210500Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
2022-12-16T14:50:00.239330Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.31'  socket: '/var/lib/mysql/mysql.sock'  port: 3306  MySQL Community Server - GPL.
2022-12-16T14:50:00.239408Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
[root@localhost mysql]# cat /var/log/mysqld.log |grep password
2022-12-16T14:49:55.960005Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: KaQMVogk9f)(

```

### 1.4.登录mysql

```bash
#进入安装目录
[root@localhost bin]# cd /usr/bin
[root@localhost bin]# ls |grep mysql
mysql
mysqladmin
mysqlbinlog
mysqlcheck
#修改用户密码
mysqladmin -u root password 'xxxxxx
mysql> alter user 'root'@'localhost' identified by 'Root@123456';
Query OK, 0 rows affected (0.01 sec)

```

###  1.5修改密码：

```bash
[root@localhost bin]# mysqladmin -uroot password 'root' -p
Enter password: 
mysqladmin: [Warning] Using a password on the command line interface can be insecure.
Warning: Since password will be sent to server in plain text, use ssl connection to ensure password safety.
[root@localhost bin]# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 15
Server version: 8.0.31 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

```



- 修改密码策略：

  关于 mysql 密码策略相关参数；
  1）、validate_password_length  固定密码的总长度；
  2）、validate_password_dictionary_file 指定密码验证的文件路径；
  3）、validate_password_mixed_case_count  整个密码中至少要包含大/小写字母的总个数；
  4）、validate_password_number_count  整个密码中至少要包含阿拉伯数字的个数；
  5）、validate_password_policy 指定密码的强度验证等级，默认为 MEDIUM；
  关于 validate_password_policy 的取值：
  0/LOW：只验证长度；
  1/MEDIUM：验证长度、数字、大小写、特殊字符；
  2/STRONG：验证长度、数字、大小写、特殊字符、字典文件；
  6）、validate_password_special_char_count 整个密码中至少要包含特殊字符的个数；

```bash
mysql> show variables like 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
mysql> set global validate_password.check_user_name =OFF;
Query OK, 0 rows affected (0.00 sec)
mysql> set global validate_password.length=4;
Query OK, 0 rows affected (0.00 sec)
mysql> set global validate_password.policy=LOW;
Query OK, 0 rows affected (0.00 sec)
mysql>  show variables like 'validate_password%';
+--------------------------------------+-------+
| Variable_name                        | Value |
+--------------------------------------+-------+
| validate_password.check_user_name    | OFF   | # 不检查用户名，只检查长度
| validate_password.dictionary_file    |       |
| validate_password.length             | 4     | # 长度最小4
| validate_password.mixed_case_count   | 1     |
| validate_password.number_count       | 1     |
| validate_password.policy             | LOW   |
| validate_password.special_char_count | 1     |
+--------------------------------------+-------+
7 rows in set (0.00 sec)

```

### 1.6 允许访问远程连接

```mysql
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed

mysql> update user set host='%' where user='root';
Query OK, 1 row affected (0.03 sec)
Rows matched: 1  Changed: 1  Warnings: 0

#刷新权限
mysql> flush privileges;
Query OK, 0 rows affected (0.06 sec)

```

开端口：

```bash
[root@localhost bin]# firewall-cmd --list-ports
8080/tcp 6379/tcp
[root@localhost bin]# firewall-cmd --add-port=3306/tcp --permanent
success
[root@localhost bin]# firewall-cmd --list-ports
8080/tcp 6379/tcp
[root@localhost bin]# firewall-cmd --reload
success
[root@localhost bin]# firewall-cmd --list-ports
8080/tcp 6379/tcp 3306/tcp

```

### 17. Mysql8密码验证方式

​     -  In MySQL 8.0, **caching_sha2_password** is the default authentication plugin rather than **mysql_native_password**.

​    -  skip-grant-tables #在my.ini，[mysqld]下添加一行，使其登录时跳过权限检查



```sql 
mysql> show global variables like '%default_aut%';
+------------------------------------------+--------------------------------+
| Variable_name                            | Value                               |
+------------------------------------------+---------------------------------+
| default_authentication_plugin | caching_sha2_password |
+------------------------------------------+----------------------------------+
1 row in set (0.01 sec)
```



- 默认密码验证方式:  caching-sha2-password

- 支持原来方式： mysql_native_password

- 修改用户密码验证方式为:

  ```mysql
  mysql> update user set plugin = 'mysql_native_password' where user = 'root';
  mysql> alter user 'root'@'192.168.56.%' identified with mysql_native_password by '12345' PASSWORD EXPIRE NEVER;
  Query OK, 0 rows affected (0.39 sec)
  
  ```

- 数据库配置修改:

  ```bash
  # vi /etc/my.cnf
  
  [mysqld]
  default_authentication_plugin = mysql_native_password
  
  ```

  

# 

### 1.8. 软链接（一般安装自动生成）

```bash
ln -s /usr/local/mysql/bin/mysql /usr/bin
ln -s /usr/local/mysql/support-files/mysql.server /usr/bin
```

### 1.9. 查看配置文件

- 配置文件目录 ： /etc/my.cnf

- 查看：

  ```bash
  [root@localhost bin]# cat /etc/my.cnf
  # For advice on how to change settings please see
  # http://dev.mysql.com/doc/refman/8.0/en/server-configuration-defaults.html
  
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
  #
  # Remove leading # to revert to previous value for default_authentication_plugin,
  # this will increase compatibility with older clients. For background, see:
  # https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_default_authentication_plugin
  # default-authentication-plugin=mysql_native_password
  
  datadir=/var/lib/mysql
  socket=/var/lib/mysql/mysql.sock
  
  log-error=/var/log/mysqld.log
  pid-file=/var/run/mysqld/mysqld.pid
  
  ```




##  二、MySQL命令行

#### 2.1. 连接数据库

```mysql
> mysql -u root -p  -- 连接数据库
mysql> show database;   -- 查看数库
mysql> use mysql  -- 选择数据库
Database changed
mysql> show tables;  -- 查看表
mysql> describe user; -- 查看表属性
```



#### 2.2. 数据类型

> 数值

- tinyint  1 byte

- smallint   2 bit

- **int 标准整数 4 bit**

- big 较大整数 8bit

- float 浮点   4 bit

- double   8bit

- decimal 字符串形式的浮点,一般金融计算使用

  

> 字符串

- char  字符串固定大小  0-255

- **varchar 可变化字符串   0-65536     常用 Java  String**

- tynytext   微型文本

- **text  文本串   2^16 -1  大文本      常用**

  

> 日期

Java.util.Date

- date:  YYYY-MM-DD
- time:  HH:mm:ss
- **datetime:  YYYY-MM-DD HH:mm:ss    常用 **
- **timestamp: 1970.1.1.到现在的毫秒数    常用 **
- year 年份

> null

- 空值，没有值

#### 2.3 字段属性

- unsigned: 无符号整数、不能申明为负数
- zerofill: 自动填充0.   不足够，如，3个长度1  -> 001
- 自增： 默认自动+1



#### 2.4 SQLyog安装

devabc 

60c1b896-7c22-4405-9f46-a6bce776ab36

 [SQLyogUltimate.zip](..\..\SwirebevUser\chennl\Downloads\SQLyogUltimate.zip) 

# 三、创建数据库

### 3.1删除数据库

```sql
DROP DATABASE IF EXISTS employees;
```



### 3.2创建数据库

```mysql
DROP DATABASE IF EXISTS employees;
CREATE DATABASE IF NOT EXISTS employees;
```

### 3.3 表操作

- 清空表数据 “truncate table 数据库名.表名”

- 删除表 “drop table 数据库名.表名”

 

```sql
DROP TABLE IF EXISTS dept_emp,
                     dept_manager,
                     titles,
                     salaries, 
                     employees, 
                     departments;
DROP TABLE IF EXISTS dept_emp,
                     dept_manager,
                     titles,
                     salaries, 
                     employees, 
                     departments;
CREATE TABLE employees (
    emp_no      INT             NOT NULL,
    birth_date  DATE            NOT NULL,
    first_name  VARCHAR(14)     NOT NULL,
    last_name   VARCHAR(16)     NOT NULL,
    gender      ENUM ('M','F')  NOT NULL,    
    hire_date   DATE            NOT NULL,
    PRIMARY KEY (emp_no)
);

CREATE TABLE departments (
    dept_no     CHAR(4)         NOT NULL,
    dept_name   VARCHAR(40)     NOT NULL,
    PRIMARY KEY (dept_no),
    UNIQUE  KEY (dept_name)
);

CREATE TABLE dept_manager (
   emp_no       INT             NOT NULL,
   dept_no      CHAR(4)         NOT NULL,
   from_date    DATE            NOT NULL,
   to_date      DATE            NOT NULL,
   FOREIGN KEY (emp_no)  REFERENCES employees (emp_no)    ON DELETE CASCADE,
   FOREIGN KEY (dept_no) REFERENCES departments (dept_no) ON DELETE CASCADE,
   PRIMARY KEY (emp_no,dept_no)
); 

CREATE TABLE dept_emp (
    emp_no      INT             NOT NULL,
    dept_no     CHAR(4)         NOT NULL,
    from_date   DATE            NOT NULL,
    to_date     DATE            NOT NULL,
    FOREIGN KEY (emp_no)  REFERENCES employees   (emp_no)  ON DELETE CASCADE,
    FOREIGN KEY (dept_no) REFERENCES departments (dept_no) ON DELETE CASCADE,
    PRIMARY KEY (emp_no,dept_no)
);

CREATE TABLE titles (
    emp_no      INT             NOT NULL,
    title       VARCHAR(50)     NOT NULL,
    from_date   DATE            NOT NULL,
    to_date     DATE,
    FOREIGN KEY (emp_no) REFERENCES employees (emp_no) ON DELETE CASCADE,
    PRIMARY KEY (emp_no,title, from_date)
) 
; 

CREATE TABLE salaries (
    emp_no      INT             NOT NULL,
    salary      INT             NOT NULL,
    from_date   DATE            NOT NULL,
    to_date     DATE            NOT NULL,
    FOREIGN KEY (emp_no) REFERENCES employees (emp_no) ON DELETE CASCADE,
    PRIMARY KEY (emp_no, from_date)
) 
;                      
```

### 3.4 查看表字段

```sql
#显示字段
SHOW COLUMNS FROM 数据表
#显示索引
SHOW INDEX FROM 数据表
#该命令将输出Mysql数据库管理系统的性能及统计信息。
SHOW TABLE STATUS [FROM db_name] [LIKE 'pattern'] \G:
```



# 四、数据库备份

### 4.1. 复制文件

### 4.2. 图形工具: SqlYog

### 4.3. 命令行： 

   ```mysql
   #导出： mysqldump -u root -p database_name table_name > filename.dump
   #如果完整备份数据库，则无需使用特定的表名称。
   mysqldump -u root -p employees department > depertments.dump
   
   #导入：将备份的数据库导入到MySQL服务器中，可以使用以下命令：
   mysql -u root -p employees < dumpfilename.txt
   password *****
   #使用以下命令将导出的数据库mysql1 直接导入到远程的服务器上mysql2
   mysqldump -u root -p database_name   | mysql -h other-host.com database_name
   ```

   

# 五、用户管理

### 5.1 创建，删除用户

```mysql
mysql> CREATE USER 'test'@'localhost' IDENTIFIED BY '123456';
mysql> SELECT `user`,`host`,`plugin`,'authentication-string' FROM USER WHERE USER='test';
mysql> DROP USER test;
```

### 5.2 用户授权：

- 创建一个普通用户并授权“grant on *.* to user1 identified by '123456';”

 ![img](https://img2020.cnblogs.com/blog/1407082/202007/1407082-20200702111458152-1276607297.png)

- all表示所有的权限（读、写、查询、删除等等操作）,

- *.*前面的*表示所有的数据库，后面的*表示所有的表

  

“grant all on 数据库名.* to 'user2'@'ip地址' identified by '密码'”

 ![img](https://img2020.cnblogs.com/blog/1407082/202007/1407082-20200702111530563-1613612844.png)

### SQL语句

```sql
mysql> CREATE USER  'peter'@'localhost' IDENTIFIED BY 'password';  
mysql> GRANT ALL PRIVILEGES ON * . * TO peter@localhost;  
mysql> GRANT CREATE, SELECT, INSERT ON * . * TO peter@localhost; 
mysql> FLUSH PRIVILEGES; 
mysql> SHOW GRANTS for username;  
```

# 六、数据库权限

 ### 6.1 mysql的两阶段验证

第一阶段：服务器首先会检查你是否允许连接。因为创建用户的时候会加上主机限制，可以限制成本地、某个IP、某个IP段、以及任何地方等，只允许你从配置的指定地方登陆。

第二阶段：如果你能连接，Mysql会检查你发出的每个请求，看你是否有足够的权限实施它。 

### 6.2 权限表

从官网复制一个表来看看：

| **权限**                | **权限级别**           | **权限说明**                                                 |
| ----------------------- | ---------------------- | ------------------------------------------------------------ |
| CREATE                  | 数据库、表或索引       | 创建数据库、表或索引权限                                     |
| DROP                    | 数据库或表             | 删除数据库或表权限                                           |
| GRANT OPTION            | 数据库、表或保存的程序 | 赋予权限选项                                                 |
| REFERENCES              | 数据库或表             |                                                              |
| ALTER                   | 表                     | 更改表，比如添加字段、索引等                                 |
| DELETE                  | 表                     | 删除数据权限                                                 |
| INDEX                   | 表                     | 索引权限                                                     |
| INSERT                  | 表                     | 插入权限                                                     |
| SELECT                  | 表                     | 查询权限                                                     |
| UPDATE                  | 表                     | 更新权限                                                     |
| CREATE VIEW             | 视图                   | 创建视图权限                                                 |
| SHOW VIEW               | 视图                   | 查看视图权限                                                 |
| ALTER ROUTINE           | 存储过程               | 更改存储过程权限                                             |
| CREATE ROUTINE          | 存储过程               | 创建存储过程权限                                             |
| EXECUTE                 | 存储过程               | 执行存储过程权限                                             |
| FILE                    | 服务器主机上的文件访问 | 文件访问权限                                                 |
| CREATE TEMPORARY TABLES | 服务器管理             | 创建临时表权限                                               |
| LOCK TABLES             | 服务器管理             | 锁表权限                                                     |
| CREATE USER             | 服务器管理             | 创建用户权限                                                 |
| PROCESS                 | 服务器管理             | 查看进程权限                                                 |
| RELOAD                  | 服务器管理             | 执行flush-hosts, flush-logs, flush-privileges, flush-status, flush-tables, flush-threads, refresh, reload等命令的权限 |
| REPLICATION CLIENT      | 服务器管理             | 复制权限                                                     |
| REPLICATION SLAVE       | 服务器管理             | 复制权限                                                     |
| SHOW DATABASES          | 服务器管理             | 查看数据库权限                                               |
| SHUTDOWN                | 服务器管理             | 关闭数据库权限                                               |
| SUPER                   | 服务器管理             | 执行kill线程权限                                             |

 

  MYSQL的权限如何分布，就是针对表可以设置什么权限，针对列可以设置什么权限等等，这个可以从官方文档中的一个表来说明：

| 权限分布 | 可能的设置的权限                                             |
| -------- | ------------------------------------------------------------ |
| 表权限   | 'Select', 'Insert', 'Update', 'Delete', 'Create', 'Drop', 'Grant', 'References', 'Index', 'Alter' |
| 列权限   | 'Select', 'Insert', 'Update', 'References'                   |
| 过程权限 | 'Execute', 'Alter Routine', 'Grant'                          |

### 6.3 授权

> 1. Grant 授权

 先来看一个例子，创建一个只允许从本地登录的超级用户jack，并允许将权限赋予别的用户，密码为：jack.

```sql
mysql> grant all privileges on *.* to jack@'localhost'  with grant option;
Query OK, 0 rows affected (0.01 sec)
```

  GRANT命令说明：
  ALL PRIVILEGES 是表示所有权限，你也可以使用select、update等权限。

  ON 用来指定权限针对哪些库和表。数据库名.

  *.* 中前面的*号用来指定数据库名，后面的*.号用来指定表名。

  TO 表示将权限赋予某个用户。

  [jack@'localhost'](mailto:jack@'localhost') 表示jack用户，@后面接限制的主机，可以是IP、IP段、域名以及%，%表示任何地方。

  IDENTIFIED BY 指定用户的登录密码。

  WITH GRANT OPTION 这个选项表示该用户可以将自己拥有的权限授权给别人。

> 2、刷新权限

```mysql
mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)
```

> 3. 查看权限

```mysql
查看当前用户的权限：
mysql> show grants;
+---------------------------------------------------------------------+
| Grants for root@localhost                                           |
+---------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION |
| GRANT PROXY ON ''@'' TO 'root'@'localhost' WITH GRANT OPTION        |
+---------------------------------------------------------------------+
2 rows in set (0.00 sec)

查看某个用户的权限：
mysql> show grants for 'jack'@'%';
+-----------------------------------------------------------------------------------------------------+
| Grants for jack@%                                                                                   |
+-----------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'jack'@'%' IDENTIFIED BY PASSWORD '*9BCDC990E611B8D852EFAF1E3919AB6AC8C8A9F0' |
+-----------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

> 4.回收权限

``` mysql
mysql> revoke delete on *.* from 'jack'@'localhost';
Query OK, 0 rows affected (0.01 sec)
```



# 七、Mysql主从原理

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Farticlecdn.lmonkey.com%2Fupload%2Fimg%2F4e23a6295b345a69fe611fb71904c28d.png&refer=http%3A%2F%2Farticlecdn.lmonkey.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1673940365&t=7208c35e8b241a4a628e97bd573c82c9)



<img src="https://img-blog.csdnimg.cn/67859de7a2274666836d03df63496f19.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbGl1c2hhbmd6YWliZWlqaW5n,size_17,color_FFFFFF,t_70,g_se,x_16#pic_center" alt="img" style="zoom: 67%;" />

如上图，从数据库备份主库数据分成三步。

1) 库开启bin-log日志: 

   主库需要设置开启记录二进制日志，提交数据更新的事务之前将更新的**事件**记录都**binlog二进制日志**中，后才进行事务提交。

2) 备库建立IO线程连接:

   备库创建IO线程与主库创建连接，主库上启动一个**binlog-dump线程**。该线程会读取主库二进制事件，该线程不会对事件进行轮询。如果从库数据与主库已经保持一致后，该线程进入休眠状态，一旦主库有新事件会以信号量唤醒binlog-dump线程。备库IO线程会将事件写入到中继日志(relay-log)中。

3)   SQL线程重放数据：

   SQL线程执行最后一步，从中继日志中获取事件并在备库中执行，从而实现数据库的更新。

   获取主库二进制事件，重放备库事件两者是解耦异步进行的。互不影响。

   

   Master在写数据时（DML命令）会产生一个bin log(Binary Log)，Slave开一个IO线程从指定偏移处去读取bin log，读回来后生成Relay Log，再开一个SQL线程"重放"数据来完成同步。
   

# 八、Docker搭建主从

### 8.0 Linux搭建主节点

##### 8.0.1 linux 直接安装节点以及配置	

```bash
#修改/etc/my.cnf:

[mysqld]

#主从参数
#该服务节点的唯一标识
server_id = 2
#开启bin-log
log_bin = /var/lib/mysql/mysql-bin
binlog-format =MIXED
#日志超过30天自动清除
expire_logs_days=30

```

#### 8.0.2 重启mysql服务 

```bash
systemctl restart mysqld
```

#### 8.0.3 查看变量

```sql
mysql> SHOW GLOBAL VARIABLES LIKE 'log_%';
+----------------------------------------+----------------------------------------+
| Variable_name                          | Value                                  |
+----------------------------------------+----------------------------------------+
| log_bin                                | ON                                     |
| log_bin_basename                       | /var/lib/mysql/mysql-bin               |
| log_bin_index                          | /var/lib/mysql/mysql-bin.index         |
| log_bin_trust_function_creators        | OFF                                    |
| log_bin_use_v1_row_events              | OFF                                             
| log_output                             | FILE                                   |
| log_queries_not_using_indexes          | OFF                                    |
| log_raw                                | OFF                                    |
| log_replica_updates                    | ON                                     |
| log_slave_updates                      | ON                                     |         
+----------------------------------------+----------------------------------------+
21 rows in set (0.00 sec)
```

#### 

### 8.1 搭建主节点-Docker:

**当前目录：**

```bash
[root@10 mysql]# pwd
/home/mysql
```

#### 8.1.1 创建容器挂载目录以及配置

```bash
## 1. data目录
[root@10 mysql]# mkdir -p master/data
## 2. log目录
[root@10 mysql]# mrdir -p master/log
## 3. 配置文件目录
[root@10 mysql]# mkdir -p master/conf
##查看
[root@10 mysql]# ls master
conf  data  log
```



##### 8.1.2 主节点配置文件

```bash
vim my.cnf
```

```ini
[client]
#设置客户端默认字符集
default-character-set=UTF8MB4

[mysqld]
#默认字符集
character-set-server = UTF8MB4

#该服务节点的唯一标识
server-id = 100
#制定复制的数据库
binlog-do-db = employees

#开启bin-log,并且设置日志文件名
log-bin = mysql-bin
#默认日志格式 row(默认) ,statement,mixed
binlog-format = statement
#设置每间隔多少次事务才会将操作写到binlog
sync-binlog = 1
#日志超过7天自动清除
binlog_expire_logs_seconds=604800
#默认使用"mysql-native-password"

```

#### 8.1.3 创建容器

```bash
##1.拉取镜像
docker pull mysql # latest mysql8.0.31

##2.运行容器
[root@SRC105036 mysql]# docker run -d  --name mysql_master \
-v /home/mysql/master/data:/var/lib/mysql \
-v /home/mysql/master/conf:/etc/mysql/conf.d \
-v /home/mysql/master/logs:/var/log/mysql \
-p3316:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:latest

8c5f938dd5cb5eafc281314385251a8e4f48868852031fb1030955c7b2895a68

##3.查看容器详情
[root@SRC105036 mysql]#docker inspect 8c5f938dd5cb5eaf
#看到挂载目录
       "Mounts": [
            {
                "Type": "bind",
                "Source": "/home/mysql/master/data",
                "Destination": "/var/lib/mysql",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "/home/mysql/master/conf/my.cnf",
                "Destination": "/etc/mysql/my.cnf",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "/home/mysql/master/log",
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
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "3316"
                    }
                ],
                "33060/tcp": null
            },
 "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",

```



#### 8.1.4 Navicat,Sqlyog连接数据库

![image-20221218181652044](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221218181652044.png)



#### 8.1.5 创建同步用户

```SQL
CREATE USER 'master'@'%' IDENTIFIED by '123456';
GRANT replication slave,replication client on *.* to 'master'@'%'; 
```

#### 8.1.6 查看master状态

```mysql
mysql> show master status;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000003 |      980 |  employees   |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set (0.06 sec)

```

**注意**:    ==日志文件名，位置。 这两个从节点接入需要配置进去==

#### 8.1.6 验证

```sql
CREATE DATABASE employees;

USE employees;

CREATE TABLE `employees`.`staff` (  
  `id` INT UNSIGNED NOT NULL,
  `name` VARCHAR(50),
  PRIMARY KEY (`id`) 
);

INSERT INTO `staff` VALUES (101,'张淑华'),(102,'李永忠'),(103,'Jack')
```



### 8.2 搭建从节点 Slave

目标： 使用docker 做从机

#### 8.2.1创建容器挂载目录以及配置

 创建挂载目录：

```bash
## 1. data目录
[root@10 mysql]# mkdir -p slave/data
## 2. log目录
[root@10 mysql]# mrdir -p slave/log
## 3. 配置文件目录
[root@10 mysql]# mkdir -p slave/conf
##查看
[root@10 mysql]# ls slave
conf  data  log
```

运行mysql  镜像，复制配置文件:

```bash
# 从容器内拷贝文件到linux主机上
# docker cp 容器id:容器内路径 目标主机路径
docker  cp eaac94ef6926:/etc/my.cnf  /home/mysql-slave/conf/my.cnf
```

在conf创建配置文件:

```bash
[root@10 conf]# vim my.cnf

skip-host-cache
skip-name-resolve
datadir=/var/lib/mysql
socket=/var/run/mysqld/mysqld.sock
secure-file-priv=/var/lib/mysql-files
user=mysql

pid-file=/var/run/mysqld/mysqld.pid

[mysqld]

pid-file=/var/run/mysqld/mysqld.pid
#master slave configration

#默认字符集
character-set-server = UTF8MB4
##服务节点id  同一局域网内注意要唯一
server-id=101
## relay_log 配置中继日志
relay_log =slave01-relay-log
[client]
socket=/var/run/mysqld/mysqld.sock


```

#### 8.2.3运行Mysql容器

```bash
docker run -d  --name mysql_slave01 -p3326:3306 -e MYSQL_ROOT_PASSWORD=123456 -v /home/mysql/slave01/data:/var/lib/mysql -v /home/mysql/slave01/conf/my.cnf:/etc/mysql/my.cnf -v /home/mysql/slave01/log:/var/log/mysql mysql:latest

```

#### 8.2.4查看容器IP,卷

```bash
docker inspect mysql-slave01
#看到挂载目录
"Mounts": [
            {
                "Type": "bind",
                "Source": "/home/mysql/slave01/data",
                "Destination": "/var/lib/mysql",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "/home/mysql/slave01/conf/my.cnf",
                "Destination": "/etc/mysql/my.cnf",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "/home/mysql/slave01/log",
                "Destination": "/var/log/mysql",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
#查看IP+端口
 "Ports": {
                "3306/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "3326"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "3326"
                    }
                ],
                "33060/tcp": null
            },
            "SandboxKey": "/var/run/docker/netns/3c08ac802019",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "7655aca2920e001b451d975ceaad1e201b090c228df3838868870160f3012388",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.3",

```

#### 8.2.4 Navicat,Sqlyog连接数据库

虚拟机上，我是通过NAT模式连接



#### 8.2.5 设置主从节点连接Master & Slave

``` mysql
## 到主机节点查看主节点状态
SHOW master status;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000003 |     2147 |  employees   |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set (0.06 sec)

## 到从机节点设置
#停止
STOP SLAVE

#连接master
change master to master_host='172.17.0.2', master_port=3306, master_user='master',  master_password='123456',  master_log_file='mysql-bin.000003',  master_log_pos=2147,master_connect_retry=30;

#开启同步
start slave
#查看结果
show slave status \G;
##关注 以下两个状态 Yes表示正常工作           
#Slave_IO_Running: Yes
#Slave_SQL_Running: Yes

mysql> show slave status \G;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for source to send event
                  Master_Host: 172.17.0.2
                  Master_User: master
                  Master_Port: 3306
                Connect_Retry: 30
              Master_Log_File: mysql-bin.000003
          Read_Master_Log_Pos: 3342
               Relay_Log_File: slave01-relay-log.000002
                Relay_Log_Pos: 326
        Relay_Master_Log_File: mysql-bin.000003
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 3342


```

![image-20221217211730349](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221217211730349.png)



#### 8.2.6 建表，插入数据验证

- 从节点创建数据库:

``` mysql
mysql> CREATE DATABASE  employees;
```

- 主机创建表：

```mysql
mysql> CREATE TABLE `employees`.`staff` (  
  `id` INT UNSIGNED NOT NULL,
  `name` VARCHAR(50),
  PRIMARY KEY (`id`) 
);

mysql> INSERT INTO `staff` VALUES (101,'张淑华'),(102,'李永忠'),(103,'Jack')
```

- 查看从节点

```sql
mysql> use employees;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+---------------------+
| Tables_in_employees |
+---------------------+
| staff               |
+---------------------+
1 row in set (0.00 sec)

mysql> select * from staff;
+-----+-----------+
| id  | name      |
+-----+-----------+
| 101 | 张淑华    |
| 102 | 李永忠    |
| 103 | Jack      |
+-----+-----------+
3 rows in set (0.00 sec)

```

#### 8.2.7查看容器日志

```bash
[root@10 conf]# docker logs mysql_master
[root@10 conf]# docker logs mysql_slave01
```

![image-20221217213837025](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221217213837025.png)


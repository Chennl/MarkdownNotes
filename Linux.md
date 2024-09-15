#  Linux

## OS&VM 安装 

1. VirtualBox

   内存 2G, 硬盘20G

2. 主机名查看修改

   - 查看主机名：

     ```bash
     hostname
     #显示主机名
     hostname
     #临时修改主机名
     hostname centos7
     #永久修改主机名 还没找到方法？？
     
     ```
     
   
   ![image-20221119153255056](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221119153255056.png)
   
   ![image-20221119163120112](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221119163120112.png)
   
3. 设置开启网络

   ``` ba
   [root@localhost ~]# cd /etc/sysconfig/network-scripts/
   [root@10 network-scripts]# ls
   ifcfg-enp0s3  ifdown-eth   ifdown-post    ifdown-Team      ifup-aliases  ifup-ipv6    
   [root@10 network-scripts]# vi ifcfg-enp0s3
   ONBOOT=YES
   ```

   ![image-20221119152721353](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221119152721353.png)

![image-20221119152849180](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221119152849180.png)

##  环境安装



### JDK - rpm方式

1. 检查版本

   ```bash
   # 查看安装的jdk   通过rpm方式安装的JDK; 如果通过解压安装，则找不到
   rpm -qa | grep java
   
   # 查看安装的jdk
   java -version
   
   #查找java 文件
   find / -name java
   
   # 卸载已有的jdk
   rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.222.b03-1.el7.x86_64
   rpm -e --nodeps java-1.8.0-openjdk-1.8.0.222.b03-1.el7.x86_64
   rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.221-2.6.18.1.el7.x86_64
   rpm -e --nodeps java-1.7.0-openjdk-1.7.0.221-2.6.18.1.el7.x86_64
   ```

   

2. 删除老版本

3. 安装

   ``` bash
   java -version
   java version "1.8.0_351"
   Java(TM) SE Runtime Environment (build 1.8.0_351-b10)
   Java HotSpot(TM) 64-Bit Server VM (build 25.351-b10, mixed mode)
   #或
   rpm -qa|grep "jdk"
   jdk1.8-1.8.0_351-fcs.x86_64
    
   ls
   anaconda-ks.cfg  jdk-8u351-linux-x64.rpm
   rpm -ivh dk-8u351-linux-x64.rpm
   warning: jdk-8u351-linux-x64.rpm: Header V3 RSA/SHA256 Signature, key ID ec551f03: NOKEY
   Preparing...                          ################################# [100%]
   Updating / installing...
      1:jdk1.8-2000:1.8.0_351-fcs        ################################# [100%]
   Unpacking JAR files...
   	tools.jar...
   	plugin.jar...
   	javaws.jar...
   	deploy.jar...
   	rt.jar...
   	jsse.jar...
   	charsets.jar...
   	localedata.jar...
   java -version
   java version "1.8.0_351"
   Java(TM) SE Runtime Environment (build 1.8.0_351-b10)
   Java HotSpot(TM) 64-Bit Server VM (build 25.351-b10, mixed mode)
   [root@localhost usr]# ls
   bin  etc  games  include  java  lib  lib64  libexec  local  sbin  share  src  tmp
   
   ```

   ![image-20221119171406071](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221119171406071.png)

4. 配置环境 /etc/profile

   ```bash
   vi /etc/profile
   source /etc/profile
   
   ```

   ![image-20221119185006242](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221119185006242.png)

### Tomcat - 解压方式

1. 解压

   ```bash
   tar -zxvf   apache-tomcat-9.0.69.tar.gz
   ```

2. 启动

   ```bash
   ./startup.sh
   ```

3. 看防火墙

   ```bash
   [root@localhost bin]# systemctl status firewalld
   ```

4. firewall命令

   - systemctl start firewalld     (过时 service firewalld start  )    
   - systemctl restart firewalld  (过时service firewall restart）
   - systemctl stop firewalld  (过时service firewall stop) 

5. 查看firewall端口

   ```bash
   firewall-cmd --list-all #查看全部信息
   firewall-cmd --list-ports #只看端口信息开启
   ```

6. 开启端口

   ```ba
   firewall-cmd --zone=public --add-port=8080/tcp --permanent

7. 开启Tomcat 8080端口

   ```bash
   firewall-cmd --list-ports
   
   firewall-cmd --zone=public --add-port=8080/tcp --permanent
   success
   systemctl restart firewalld
   firewall-cmd --list-ports
   8080/tcp

8. 外部机器 访问Tomcat,成功！（本机验证curl http://localhost:8080）

   ![image-20221119212446021](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221119212446021.png)

		### Docker-Yum方式

docker 官网安装文档: https://docs.docker.com/engine/install/centos/

> 安装
>
> 1. 检查CentOS 7
>
>    ```` ba
>    cat /etc/redhat-release
>    CentOS Linux release 7.9.2009 (Core)
>    yum -y install gcc
>    yum -y install gcc-c++
>                                              
>    ````
>
>    2.官网安装步骤：
>
>    ``` Ba
>    #删除已有版本
>    yum remove docker \
>                      docker-client \
>                      docker-client-latest \
>                      docker-common \
>                      docker-latest \
>                      docker-latest-logrotate \
>                      docker-logrotate \
>                      docker-engine
>                                                                
>    yum install -y yum-utils
>    
>    
>    ```
>
>    详细见Docker 笔记

# 常用命令

##  1.防火墙命令

```bash
#开启防火墙
systemctl restart firewalld.service
#查看开放了哪些端口
firewall-cmd --list-all
#查看开放的端口
firewall-cmd --list-ports

#yes：代表开放，no：没开发
firewall-cmd --query-port=3306/tcp
yes 


#如果没有显示80，则需要开放80端口
firewall-cmd --add-port=80/tcp --permanent #开放端口 
firewall-cmd --remove-port=80/tcp --permanent
firewall-cmd --reload #重新加载
```

 

## 2.删除mysql安装包

### 1. yum检查

```shell
yum list installed | grep mysql
```

有安装则直接删除，删除掉你自己查询后显示的安装包，示例如下：

```shell
yum remove mysql mysql-server mysql-libs compat-mysql
yum remove mysql-community-release
```

注意: 再次执行检查命令，有则删除，直到查询不出结果

### 2. rpm检查

```shell
rpm -qa | grep -i mysql 
```

有则直接删除，删除掉你自己查询后显示的安装包，示例如下：

```shell
rpm -e --nodeps mysql-community-libs-5.7.22-1.el7.x86_64
rpm -e –nodeps mysql57-community-release-el7-11.noarch
```

注意: 再次执行检查命令，有则删除，直到查询不出结果

### 3.删除mysql安装目录和残存文件

```shell
whereis mysql
find / -name mysql
```

将以上命令运行查找到的结果，全部rm -r -f 删除

### 4.删除mysql配置文件

my.cnf为例，一般在/etc/my.cnf，直接rm即可
如果设置了开机启动，也需要关闭

> chkconfig --list | grep -i mysql
> chkconfig --del mysqld

### 5.卸载MariaDB

```bash
yum remove mariadb
#卸载mariadb，同时也卸载了mariadb-server
yum list installed | grep mariadb
#发现在安装mariadb时作为依赖项的mariadb-libs没有被删除。
yum remove mariadb-libs
#将其卸载
rm -rf /etc/my.cnf
rm -rf $(find / -name mysql)
#删除所有包含mysql的文件（夹）
```

## 3.Linux软件安装方式

1.1软件安装方式
在Linux系统中， 安装软件的方式主要有四种，这四种安装方式的特点如下：

| 安装方式     | 特点                                                         |
| ------------ | ------------------------------------------------------------ |
| 二进制发布   | 软件已经针对具体平台编译打包发布，只要解压，修改配置即可     |
| rpm安装      | 软件已经按照redhat的包管理规范进行打包，使用rpm命令进行安装，==不能自行解决库依赖问题== |
| yum安装      | 一种在线软件安装方式，本质上还是rpm安装，自动下载安装包并安装，安装过程中自动解决库依赖问题(安装过程需要联网) |
| 源码编译安装 | 软件以源码工程的形式发布，需要自己编译打包                   |

## 4.应用后台运行方式

后台运行程序:

要想让我们部署的项目进行后台运行，这个时候我们需要使用到linux中的一个命令 nohup ，接下来，就来介绍一下nohup命令。

==**nohup**==命令：英文全称 no hang up（不挂起），用于不挂断地运行指定命令，退出终端不会影响程序的运行

语法格式： nohup Command [ Arg … ] [&]

参数说明：

Command：	要执行的命令

Arg：					一些参数，可以指定输出文件

&：					让命令在后台运行

举例：

nohup java -jar boot工程.jar & > hello.log &

上述指令的含义为： 后台运行 java -jar 命令，并将日志输出到 hello.log 文件

## 5.主机名与网络

### 5.1 查看与修改主机名

```bash 
## 1. 查看主机名
[root@slave1 /]# uname
Linux
[root@slave1 /]# hostname
slave1
##2. 临时修改主机名；重新登录生效；重启后失效
[root@slave1 /]# hostname  slave1
##3.1永久修改主机名,永久生效
[root@slave1 /]# hostnamectl set-hostname  salve01 
##3.2直接修改文件
[root@master ~]# cat /etc/hostname
master
[root@master ~]# vim /etc/hostname
##4 查看主机情况
[root@master ~]# hostnamectl status
   Static hostname: master
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 73ecf4deabb84b1c93cfdf695c111889
           Boot ID: e674fe5ab6da4dcabfa166c1172a8d6a
    Virtualization: vmware
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-1160.el7.x86_64
      Architecture: x86-64

```



### 5.2 语言、时间与日期

```bash
 ##1.语言设置
[root@master ~]# localectl
   System Locale: LANG=en_US.UTF-8
       VC Keymap: us
      X11 Layout: us
 
 ##2.1 查看时区，时间，日期
 [root@master ~]# date
Thu Dec 22 05:19:58 EST 2022

[root@master ~]# timedatectl
      Local time: Thu 2022-12-22 05:17:46 EST
  Universal time: Thu 2022-12-22 10:17:46 UTC
        RTC time: Thu 2022-12-22 10:17:46
       Time zone: America/New_York (EST, -0500)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: no
      DST active: no
 Last DST change: DST ended at
                  Sun 2022-11-06 01:59:59 EDT
                  Sun 2022-11-06 01:00:00 EST
 Next DST change: DST begins (the clock jumps one hour forward) at
                  Sun 2023-03-12 01:59:59 EST
                  Sun 2023-03-12 03:00:00 EDT
##2.2
[root@master ~]# date -R
Thu, 22 Dec 2022 18:24:12 +0800

##3. 设置正确时区
[root@master ~]# timedatectl set-timezone Asia/Shanghai

检查并重新配置时区
sudo dpkg-reconfigure tzdata


##4. 启动时间同步
sudo timedatectl set-ntp true
###5.检查和修复硬件时钟
sudo hwclock --systohc --localtime



```

###5.2 查看当前登录用户

```bash
## 1.
[root@master ~]# loginctl
   SESSION        UID USER             SEAT            
         1          0 root                             
        17          0 root                             
2 sessions listed.
##2.
[root@master ~]# w
 18:27:30 up  2:23,  2 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    192.168.13.1     16:04    2:22m  0.02s  0.02s -bash
root     pts/1    192.168.13.1     18:13    2.00s  0.06s  0.01s w
[root@master ~]# 

```

## 6.网卡相关

### 6.1命名方式：

centos7： ens[33,34;35,....]

### 6.2启停网卡

```Bash
##关闭网卡
[root@master ~]# ifdown
usage: ifdown <configuration>
##开启网卡
[root@master ~]# ifup
Usage: ifup <configuration>
##重启网络
[root@master ~]# systemctl restart network

```

### 6.3 host与IP映射

```bash 
##查看 
[root@master ~]# getent hosts
127.0.0.1       localhost localhost.localdomain localhost4 localhost4.localdomain4
127.0.0.1       localhost localhost.localdomain localhost6 localhost6.localdomain6
##查看 
[root@master ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
##编辑
[root@master ~]# vim /etc/hosts

```

### 6.4配置网络  

网络配置文件:

> /etc/sysconfig/network-scripts

```bash
[root@master network-scripts]# pwd
/etc/sysconfig/network-scripts
[root@master network-scripts]# ls
ifcfg-ens33  ifdown-ippp  ifdown-routes    ifup          ifup-ipv6   ifup-ppp       ifup-tunnel
ifcfg-lo     ifdown-ipv6  ifdown-sit       ifup-aliases  ifup-isdn   ifup-routes    ifup-wireless
ifdown       ifdown-isdn  ifdown-Team      ifup-bnep     ifup-plip   ifup-sit       init.ipv6-global
ifdown-bnep  ifdown-post  ifdown-TeamPort  ifup-eth      ifup-plusb  ifup-Team      network-functions
ifdown-eth   ifdown-ppp   ifdown-tunnel    ifup-ippp     ifup-post   ifup-TeamPort  network-functions-ipv6
[root@master network-scripts]# vim ifcfg-ens33
TYPE="Ethernet" 
PROXY_METHOD="none" 
BROWSER_ONLY="no" 

BOOTPROTO="dhcp" #改成静态
DEFROUTE="yes" 
IPV4_FAILURE_FATAL="no" 
IPV6INIT="yes" 
IPV6_AUTOCONF="yes" 
IPV6_DEFROUTE="yes" 
IPV6_FAILURE_FATAL="no" 
IPV6_ADDR_GEN_MODE="stable-privacy" 
NAME="ens33" 
UUID="715d6548-0ea0-4e24-93f0-416334ef0986" 
DEVICE="ens33" 
ONBOOT="yes"

```
### 6.5 设置静态IP

```sh
#参考： 
https://blog.csdn.net/y2020520/article/details/131177497

```



## 7 用户管理

### 7.1用户命令

```shell
## 1、添加用户
#useradd 用户名   会自动创建与用户同名的家目录，在/home/ 下面
[root@master ~]# useradd chennl

## 2、查询用户
#id 用户名       uid:用户id； gid:用户所在组id；groups:组名
[root@master ~]# id chennl
uid=1000(chennl) gid=1000(chennl) groups=1000(chennl)

##3.1 修改/指定用户密码
[root@master ~]# passwd chennl
    Changing password for user chennl.
    New password: 
    BAD PASSWORD: The password is shorter than 8 characters
    Retype new password: 
    passwd: all authentication tokens updated successfully.
#3.2 如果没有带用户名，则是给当前登录的用户修改密码
[root@master ~]# passwd
    Changing password for user root.
    New password: 
    BAD PASSWORD: The password is shorter than 8 characters
    Retype new password: 
    passwd: all authentication tokens updated successfully.

##4、切换用户  su - 用户名
#4.1.从权限高的用户切换到权限低的用户，不需要输入密码，反之需要。
#4.2. 当需要返回到原来用户时，使用 exit 指令
#4.3. 如果 su – 没有带用户名，则默认切换到 root 用户
[root@master ~]# su chennl
[chennl@master root]$ su root
Password: 

##5、删除用户
#5.1）userdel 用户名  删除用户，但是需要保留家目录
#5.2）userdel -r 用户名
[root@master ~]# useradd test
[root@master ~]# userdel -r   test


```


### 7.2 用户组

```shell
# 1、增加组    groupadd 组名

# 2、修改组 usermod –g 新的组名 用户名

# 3、删除组 groupdel 组名  // 这个组没有用户，才能删除
```



### 7.3 用户与组相关的文件

**1、/etc/passwd 文件 **
用户（user）的配置文件，记录用户的各种信息

每行的含义：用户名:口令:用户标识号:组标识号:注释性描述:主目录
**2、/etc/shadow 文件**
口令的配置文件 ，

每行的含义：登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:
**3、/etc/group 文件**
组(group)的配置文件，记录 Linux 包含的组的信息，

每行含义：组名:口令:组标识号:组内用户列表



### 7.4 实战

```sh
#创建目录
#当前用户：root
[root@localhost ~]# mkdir pycode
#创建用户组: python开发组
[root@localhost ~]# groupadd pydev
#更改pycode组为 pydev
[root@localhost ~]# ll
drwxrwxr-x. 2 root   root   22 Jul 10 16:55 pycode
[root@localhost ~]# chgrp pydev  pycode
[root@localhost ~]# chown root:pydev pycode
[root@localhost ~]# ll
drwxr-xr-x. 2 root   pydev   22 Jul 10 16:55 pycode
#给pydev组 有创建，修改和删除文件的权限
[root@localhost ~]# chmod g+rwx pycode
[root@localhost ~]# ll
drwxrwxr-x. 2 root   pydev   22 Jul 10 16:55 pycode

#将用户 chennl加入pydev 组

[root@localhost ~]# usermod --help
  -g, --gid GROUP               force use GROUP as new primary group
  -G, --groups GROUPS           new list of supplementary GROUPS
  -a, --append                  append the user to the supplemental GROUPS
                                mentioned by the -G option without removing
                                the user from other groups
#更改主用户组 -g
[root@localhost ~]# usermod -g chennl chennl

#将用户添加到另外用户组 -a -G
[root@localhost ~]# usermod -a -G pydev chennl

[root@localhost ~]# id chennl
uid=1000(chennl) gid=1000(chennl) groups=1000(chennl),1003(pydev)

## 使用gpasswd命令
[root@localhost ~]# gpasswd --help
Usage: gpasswd [option] GROUP
Options:
  -a, --add USER                add USER to GROUP
  -d, --delete USER             remove USER from GROUP
  
[root@localhost ~]# gpasswd -a chennl sshd
Adding user chennl to group sshd
[root@localhost ~]# gpasswd -d chennl sshd
Removing user chennl from group sshd

```










## 常用命令

**linux查看端口占用的方法：**

1、常用命令：

（1）lsof -i 端口号

（2）netstat -tunlp|grep 端口号 

### 1. Linux中查看进程 

process status = ps

```sh
# 查看活跃线程
ps -ef 查看所有活跃进程
ps -ef|grep *** 查看某个进程
[root@nodeone ~]# ps -ef|grep mysql
mysql       1254       1  0 04:13 ?        00:00:11 /usr/sbin/mysqld
root        1463    1402  0 04:42 pts/1    00:00:00 grep --color=auto mysql
# ps -l 列出与 本次登录系统 有关的进程信息

[root@nodeone ~]# ps -l
F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S     0    1402    1381  0  80   0 -  2172 do_wai pts/1    00:00:00 bash
4 R     0    1466    1402  0  80   0 -  2525 -      pts/1    00:00:00 ps

#ps -aux 列出在内存中运行的 全部进程信息
#ps -aux | grep *** 列出 某个 进程的详细信息
[root@nodeone ~]# ps -aux | grep mysql
mysql       1254  0.6 26.2 1841804 520576 ?      Ssl  04:13   0:12 /usr/sbin/mysqld
root        1468  0.0  0.1   6408  2176 pts/1    S+   04:46   0:00 grep --color=auto mysql

#杀死进程 kill -9 pid
kill -9 pid
```

### 2. Linux中查看日志

#### tail 参数

查看看日志参数：

`-f`循环读取

`-q`不显示处理信息

`-v`显示详细的处理信息

`-c<数目>` 显示的字节数

`-n<行数>` 显示行数

`--pid=PID` 与-f合用,表示在进程ID,PID死掉之后结束.

`-q, --quiet, --silent`从不输出给出文件名的首部

`-s, --sleep-interval=S` 与 `-f` 合用,表示在每次反复的间隔休眠S秒

 

```sh
# 
[root@nodeone ~]# tail -f /var/log/mysqld.log

[root@nodeone ~]#  tail -n 200 -f dpe_partner_all.log | perl -pe 's/(关键词)/\e[1;31m$1\e[0m/g'
```


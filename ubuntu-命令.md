# ubuntu-18.04 操作命令
---
## 查看版本号:
``` lsb_release -cs ```
## 常用命令：
 - 升级安装包：```  sudo apt update``` 
 - 删除一个空目录
``` rm -d 目录名```
- 删除一个空目录
``` rm -dir 目录名``` 
- 删除一个非空目录
``` rm -r 目录名``` 
- 删除文件
``` rm 文件名``` 
## ubuntu常见的关机命令和重启命令:
 - 关机命令：
    + 立刻关机：``` sudo shutdown -h now ```
          sudo init 0
          poweroff
    + 延时关机：
        sudo shutdown -h 10 [“准备关机”] //10分钟后关机。方括号表示可选，用于在关机前提醒
        shutdown -c 取消关机
    + 系统重启命令
        立刻重启
        sudo shutdown -r now
        sudo reboot
        延时重启：
        sudo shutdown -r 10 //10分钟后重启
        shutdown -c 取消重启
## 查检安装包:
  - dpkg -s firefox
  - apt-cache search java  //在源软件列表中查找相应的软件包
  - #apt-cache depends java   //查找fping依赖哪些包
  - #apt-get rdepends java   //查找哪些包依赖fping
## Linux 用户

 - 看到全部用户:
 ``` sudo cat /etc/passwd ```
 ```
1 root:x:0:0:root:/root:/bin/bash
2 daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
3 bin:x:2:2:bin:/bin:/usr/sbin/nologin
4 sys:x:3:3:sys:/dev:/usr/sbin/nologin
5 sync:x:4:65534:sync:/bin:/bin/sync
6 games:x:5:60:games:/usr/games:/usr/sbin/nologin
7 man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
8 lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
9 mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
 ```

 /etc/passwd 文件的每一行信息，代表一个用户，我们自己也能在这个文件按照上面的格式，写入这样的一行信息，也就相当于新增了一个用户。Linux 的所有一切，都可以通过修改一个文件改变，这就正是我们开篇提到的 一切皆文件 的概念。

在每一行信息中又通过 : 号分割成七个部分，每个部分又代表了不同的含义。

1. 用户名。
2. 密码，上面都是用 x 字符表示，其实它的真身保存在另外一个文件 /etc/shadow，是加密的状态。
3. uid (用户编号)，500 以内的 ID 已经被系统保留了，我们注册的普通用户一般都大于 500 或者 1000。
4. gid (用户组编号)，存放在单独管理用户组表 /etc/group。
5. 注释。
6. 用户目录。
7. shell 默认是 bash。
## 创建与删除用户
虽然通过 /etc/passwd 文件能直接操作用户，但是这种方式有点原始古老，也容易出错，更常见的是通过 命令操作。

创建用户的第一步需要先创建用户组，针对不同的组我们后面可以设定不同的 权限管理。
```
// 创建组
$ groupadd gname

// 删除组
$ groupdel gname

// 查看组ID
$ egrep 'gname' /etc/group

// 创建用户
$ useradd -g gid uname 



// 删除用户
$ userdel uname
```
- 刚刚创建好的用户还不能使用，需要为其添加一个密码
``` $ passwd uname // 激活用户，给用户设置一个密码 ```

- su  用户之间的切换。
``` $> sudo su // 切换到 root;      $> su jmjc // 普通用户切换 ```
## linux用户添加到多个组
 - ``` usermod -G groupname username``` (这种会把用户从其他组中去掉，只属于该组)
如：``` usermod -G git git``` (git只属于git组)

``` usermod -a -G groupname username ``` (把用户添加到这个组，之前所属组不影响)
如：``` usermod -a -G www git`` (git属于之前git组，也属于www组)

## 查用户属于哪些组
``` groups 用户名```
## 查 adm组有哪些用户
``` egrep adm  /etc/group ```
## python3
- 虚拟环境
``` python3 -m venv venv``` 
- 激活
``` source venv/bin/activate``` 
- 安装模块
``` pip install Flask``` 
- 退出虚拟环境
``` deactivate``` 
### 通过ps，查看uwsgi相关进程
``` ps aux|grep uwsgi ```
### kill pid会发送SIGTERM，只会导致重启，而不是结束掉。需要发送SIGINT或SIGQUIT，对应着是INT才可以
``` killall -s INT /usr/local/bin/uwsgi ```

### 根据端口先查出端口对应的进程号，然后杀掉进程

``` lsof -i :8001   ```  # 根据端口后进行查询 查询结果中的PID就是进程号,如果相关进程有多个，那就杀多个
``` sudo kill -9 进程号  ``` #根据进程号杀死进程

## Linux [文件权限] (https://www.jmjc.tech/less/130)
chmod:
chmod 755 file // 得到的结果就是 rwxr-xr-x，每个数字代表一组 | 4读、2写、1执行，如果全部权限都赋予，合起来就是 7 </br>
chmod 333 file // -wx-wx-wx | 加深理解

chogrp gname file // 修改文件的所属用户组</br>
chown uname file // 修改文件的所属用户
## 软链接
链接是文件的一种的镜像，访问一个文件的指针指向了两一个文件。
``` ln -s a b```
上面的命令，新创建了一个 b 的文件，并且链接到 a。它们之间的关系，修改 b 等于了 修改 a，修改 a 等于修改了 b。但是 b 文件依赖着 a，假如 a 文件不在了，b文件也就不能访问。




## 安装MySQL:  
若要在 WSL 上安装 MySQL (Ubuntu 18.04) ：
1. 打开 WSL 终端（即 Ubuntu 18.04）。
2. 更新 Ubuntu 包：sudo apt update
3. 更新包后，使用以下内容安装 MySQL： sudo apt install mysql-server
4. 确认安装并获取版本号：mysql --version

你可能还需要运行包含的安全脚本。 这会更改远程根登录名和示例用户之类的一些安全默认选项。 
运行安全脚本：
1. 启动 MySQL server： sudo /etc/init.d/mysql start
2. 启动安全脚本提示： sudo mysql_secure_installation
3. 第一个提示将询问你是否要设置 "验证密码" 插件，该插件可用于测试 MySQL 密码的强度。 然后，你将为 MySQL 根用户设置密码，决定是否要删除匿名用户，决定是否允许根用户本地和远程登录，确定是否要删除测试数据库，最后确定是否要立即重新加载特权表。

- 打开 MySQL 提示符，请输入： sudo mysql
- 查看可用数据库，请在 MySQL 提示符下输入： SHOW DATABASES;
- 创建新数据库，请输入： CREATE DATABASE database_name;
- 删除数据库，请输入： DROP DATABASE database_name;
- 有关使用 MySQL 数据库的详细信息，请参阅 [mysql](https://dev.mysql.com/doc/mysql-getting-started/en/) 文档。
- 若要在 VS Code 中使用 MySQL 数据库，请尝试 [mysql 扩展](https://marketplace.visualstudio.com/items?itemName=cweijan.vscode-mysql-client2)。
   
## 将WSL2作为生产力工具
https://www.cnblogs.com/dmego/p/12082013.html
https://www.cnblogs.com/panlq/p/13704965.html


 




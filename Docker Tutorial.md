# Docker

> Docker学习

- Docker概述

- Docker安装

- Docker命令

  - 镜像命令
  - 容器命令
  - 操作命令
  - ...

- Docker 镜像

- 容器数据卷

- DockerFile

- Docker网络原理

- IDEA 整合Docker

- **Docker Compose(集群)**

- **Docker Swarm**

- **CI\CD Jenkins**

# Docker 概述

#  Docker 的历史

Docker 是基于Go语言开发的开源项目

官网：https://www.docker.com

仓库地址：https://hub.docker.com



# Docker 基本架构图

![查看源图像](https://ts1.cn.mm.bing.net/th/id/R-C.09c32c53ac3e35fb8f94d88367f8ee95?rik=eM8%2br4OeZNAOQg&riu=http%3a%2f%2ftestingpai.com%2fupload%2ffile%2f2020%2f12%2f2-d485df03.jpg&ehk=oloyZT%2f1jV1ioh3faelPOnsm0PLV7P0ud%2fq5qskOne8%3d&risl=&pid=ImgRaw&r=0)

- 镜像 Image

- 容器 Container

# Docker 安装  

##  安装

  官方文档：[Install Docker Engine on CentOS | Docker Documentation](https://docs.docker.com/engine/install/centos/)

docker Hub国内摘取：

1. ### Uninstall old versions

   ```bash
   yum remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
   ```

   

2. #### Set up the repository

   ```bash
   #需要安装包：
   yum install -y yum-utils
   #官方
   yum-config-manager \
       --add-repo \
       https://download.docker.com/linux/centos/docker-ce.repo
    #阿里(推荐)
    yum-config-manager  --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    #更新Yum软件包索引
    yum makecache fast
   ```

   ```
   #vim /etc/NetworkManager/NetworkManager.conf
   # vim /etc/sysconfig/network-scripts/ifcfg-ens33 
   ```

   

3. Install Docker Engine

   ```bash
    yum -y install docker-ce docker-ce-cli containerd.io
    
   ```

4. Start Docker.

```bash
systemctl start docker
systemctl restart docker
systemctl daemon-reload
```

5. Verify that Docker Engine is installed correctly by running the hello-world image.

   ```bash
   docker version  #查看Docker版本
   docker run hello-world
   ```

6. 查看镜像

   ```ba
   docker images
   ```

    

## 卸载

   1. Uninstall the Docker Engine, CLI, Containerd, and Docker Compose packages:

      ```bash
      yum remove docker-ce docker-ce-cli containerd.io
      ```

      

   2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

      ```bash
      rm -rf /var/lib/docker  #docker默认工作路径
      rm -rf /var/lib/containerd
      ```


# Docker 命令

## 

```bash
#后台启动命令 docker run -d 镜像名
docker run -d centos
#查看日志 docker logs -tf -tail 10 镜像名
docker logs -tf -tail 10 centos
#查看进程 docker ps -a
docker ps 
#查看容器内部进程信息
# docker top 容器ID

# 查看镜像 原数据 docker inspect ID
docker inspect id
#进入当前正在运行的容器, 进入新的终端
docker exec -it 容器ID /bin/bash
#docker attach 容器id， 进入正在执行的终端
docker attach id
##删除运行过的容器
docker rm ID

#删除所有镜像
docker rmi `docker images -q`
#停止所有容器
docker stop $(docker ps -qa)
docker rm $(docker ps -qa)  #这个命令不能删除运行中的容器
```

**退出容器，但不关闭容器的方法**

按“**Ctrl+P+Q**”按钮退出容器，即可正常退出不关闭容器；

 

**从容器内拷贝到主机**

```bash
[root@10 ~]# docker run -it  5d0da3dc9764
[root@e30e3820727a /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@e30e3820727a /]# cd /home 
[root@e30e3820727a home]# ls
[root@e30e3820727a home]# touch test.java
[root@e30e3820727a home]# exit
exit
[root@10 ~]# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED              STATUS                      PORTS     NAMES
e30e3820727a   5d0da3dc9764   "/bin/bash"   About a minute ago   Exited (0) 6 seconds ago              intelligent_ardinghelli
[root@10 ~]# docker cp e30e3820727a:/home/test.java /home
[root@10 ~]# ls /home
chennl  mysql  test.java

```

**从主内拷贝到机容器**

一般使用挂载方式

## 安装Nginx

```bash
#查找 nginx 建议在 https://hub.docker.com/搜索
docker search nginx
docker pull nginx
docker run --name nginx01 -p 3344:80 nginx   # 3344是宿主机端口 80是容器内nginx端口
#在另外一个终端中查看
docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
41a8d45247b9   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:3344->80/tcp, :::3344->80/tcp   nginx01
# 在主机访问nginx测试
[root@10 ~]# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
#进入nginx容器

[root@10 ~]# docker exec -it nginx01 /bin/bash
root@41a8d45247b9:/# wheris nginx
bash: wheris: command not found
root@41a8d45247b9:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@41a8d45247b9:/# exit
exit
[root@10 ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
41a8d45247b9   nginx     "/docker-entrypoint.…"   19 minutes ago   Up 19 minutes   0.0.0.0:3344->80/tcp, :::3344->80/tcp   nginx01
[root@10 ~]# docker stop 41a8d45247b9
41a8d45247b9
 #这时候ngix停止，不能被访问

```

![image-20221112170115598](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221112170115598.png)

## 安装Tomcat

```bash
#官网例子
docker run -it --rm tomcat:9.0   #使用完删除镜像
#正常安装
docker pull tomcat:9.0.68
[root@10 ~]# docker pull tomcat:9.0.68
9.0.68: Pulling from library/tomcat
e96e057aae67: Pull complete 
014fa72e018d: Pull complete 
06768b8afb03: Pull complete 
3c12ca51ab80: Pull complete 
55a6d794ff88: Pull complete 
6dc9d0a6959a: Pull complete 
dcacbd9ae58f: Pull complete 
Digest: sha256:08b35bf6567527a3532bbf5a720f580906ce3b9b6cb4a46cac04b0a841f4de8c
Status: Downloaded newer image for tomcat:9.0.68
docker.io/library/tomcat:9.0.68
#启动运行
[root@10 ~]# docker run -p 3355:8080 --name tomcat01 tomcat:9.0.68
NOTE: Picked up JDK_JAVA_OPTIONS:  --add-opens=java.base/java.lang=

#测试访问没有问题，但，报错
#进入容器
docker exec -it tomcat01 /bin/bash
root@0449fd36d248:/usr/local/tomcat# ls
bin           CONTRIBUTING.md  logs            README.md      temp          work
BUILDING.txt  lib              native-jni-lib  RELEASE-NOTES  webapps
conf          LICENSE          NOTICE          RUNNING.txt    webapps.dist
root@0449fd36d248:/usr/local/tomcat# cd webapps
#发现没有管理app
root@0449fd36d248:/usr/local/tomcat/webapps# ls
root@0449fd36d248:/usr/local/tomcat/webapps# 
#从webapps.dist复制Tomcat管理页面
root@0449fd36d248:/usr/local/tomcat# cp -r ./webapps.dist/* webapps/
root@0449fd36d248:/usr/local/tomcat# ls webapps
docs  examples  host-manager  manager  ROOT
#刷新浏览器，显示正常
```

![image-20221112174157775](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221112174157775.png)

## 可视化面板安装Portainer

- Portainer

- Rancher

  

### Portainer

Portainer 是一个用于管理容器化应用程序的开源工具.

```bash
#通过命令安装 Portainer

docker run -d -p 8088:9000 -v /var/run/docker.sock:/var/run/docker.sock --privileged portainer/portainer

[root@10 ~]# docker run -d -p 8088:9000 -v /var/run/docker.sock:/var/run/docker.sock --privileged portainer/portainer
Unable to find image 'portainer/portainer:latest' locally
latest: Pulling from portainer/portainer
772227786281: Pull complete 
96fd13befc87: Pull complete 
bdbe4b3ab581: Pull complete 
60b25cb35342: Pull complete 
Digest: sha256:471457e97268f6f71e883158ebd9d3da020c7263cdba06881473d8b69bfe2d4a
Status: Downloaded newer image for portainer/portainer:latest
d5d24d185ce10e46806498332814578e9ca3c817c1b159d84bb4a1cfe63da2a3
#访问测试

 
```

![image-20221112184427142](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221112184427142.png)

user: admin

pwd: Happynewyear!

![image-20221112184626299](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221112184626299.png)
# Docker镜像加载原理

## 1、镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时环境、库、环境变量和配置文件。

即：所有的应用，直接打包成Docker镜像，就可以直接跑起来！

## 2、Docker镜像获取的方式

从仓库中拉取镜像（docker pull）。
从本地文件中载入镜像（docker load）。
由容器生成新的镜像（docker commit）。
自己构建新的镜像（docker build）。

## 3、Docker镜像加载原理

（1）UnionFS（联合文件系统）
Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交，来一层层的叠加。
同时可以将不同目录，挂载到同一个虚拟文件系统下（unite several directories into a single virtual filesystem）。
Union文件系统是Docker镜像的基础。
镜像可以通过分层来进行继承，基于基础镜像（没有父镜像概念），可以制作各种具体的应用镜像。
 2）Docker镜像加载原理
Docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS（联合文件系统）。

分为两个部分：

bootfs（boot file system）：主要包含bootloader和kernel（Linux内核），bootloader主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，而在Docker镜像的最底层也是bootfs这一层，这与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后，整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

即：系统启动时需要的引导加载，这个过程会需要一定时间。就是黑屏到开机之间的这么一个过程。电脑、虚拟机、Docker容器启动都需要的过程。在说回镜像，所以这一部分，无论是什么镜像都是公用的。

rootfs（root file system）：rootfs在bootfs之上。包含的就是典型Linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。

即：镜像启动之后的一个小的底层系统，这就是我们之前所说的，容器就是一个小的虚拟机环境，比如Ubuntu，Centos等，这个小的虚拟机环境就相当于rootfs。
 ![img](https://img-blog.csdnimg.cn/0ea8e92d4a7f476290e7ae335f21f53b.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2plcnJ5X2xpdWZlbmc=,size_16,color_FFFFFF,t_70)

> 平时我们安装进虚拟机的CentOS系统都是好几个G，为什么Docker这里才200M？

![image-20221112185811093](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221112185811093.png)

对于一个精简的OS系统，rootfs可以很小，只需要包含最基本的命令、工具和程序库就可以了，因为底层直接用Host（宿主机）的kernel（也就是宿主机或者服务器的boosfs+内核），自己只需要提供rootfs就可以了。

由此可见对于不同的linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs部分。

这就是我们之前说：虚拟机的启动是分钟级的，容器的启动是秒级的

## 4.镜像分层理解
### （1）docker分层的镜像

多个镜像都是从相同的base镜像构建的，宿主机只需要在磁盘上保留一份Base镜像，同时内存中也只需要加载一份Base镜像，就可以应用在所有的容器服务之中了。而且镜像的每一层都可以被共享
使用 docker image inspect可以查看镜像元数据

[root@192 /]# docker image inspect redis:latest
[
    {
        "Id": "sha256:621ceef7494adfcbe0e523593639f6625795cc0dc91a750629367a8c7b3ccebb",
        "RepoTags": [
            "redis:latest"
        ],
        ... # 省略
        ... # 省略
        "RootFS": {
            "Type": "layers",
            "==Layers==": [ 
                "sha256:cb42413394c4059335228c137fe884ff3ab8946a014014309676c25e3ac86864",
                "sha256:8e14cb7841faede6e42ab797f915c329c22f3b39026f8338c4c75de26e5d4e82",
                "sha256:1450b8f0019c829e638ab5c1f3c2674d117517669e41dd2d0409a668e0807e96",
                "sha256:f927192cc30cb53065dc266f78ff12dc06651d6eb84088e82be2d98ac47d42a0",
                "sha256:a24a292d018421783c491bc72f6601908cb844b17427bac92f0a22f5fd809665",
                "sha256:3480f9cdd491225670e9899786128ffe47054b0a5d54c48f6b10623d2f340632"
            ]
        },
        ... # 省略
    }
]

### （2）加深理解
  所有的Docker镜像都起始于一个基础镜像层，当进行修改或增加新的内容时，就会在当前镜像层之上，创建新的镜像层。
  举一个简单的例子，假如基于Ubuntu Linux 16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加Python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创建第三个镜像层。
  该镜像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/27d85a0ebd7b42368f41bb578babbcca.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2plcnJ5X2xpdWZlbmc=,size_16,color_FFFFFF,t_70)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了一个简单的例子，每个镜像层包含3个文件，而整体的镜像包含了来自两个镜像层的6个文件。

![在这里插入图片描述](https://img-blog.csdnimg.cn/80813f9132d64346b8a78ddba127b381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2plcnJ5X2xpdWZlbmc=,size_16,color_FFFFFF,t_70)

上图中的鏡像层跟之前图中的略有区别，主要目的是便于展示文件。

下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版本。

![在这里插入图片描述](https://img-blog.csdnimg.cn/b459491ccd1147818a4af5ae8151d9fc.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2plcnJ5X2xpdWZlbmc=,size_16,color_FFFFFF,t_70)

 这种情况下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中。
  Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统。

如上边的三层镜像，Docker最终会把所有镜像层堆叠并合并，对外提供统一的视图，如下图
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/5a61f2ea34a843e485a4a34d48bca926.png)

![image-20221112190955299](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221112190955299.png)

### （5）commit镜像

```bash
#查看images
docker images
#启动默认tomcat容器
docker run -it -p 8080:8080 tomcat:9.0.68
#进入tomcat
[root@10 ~]# docker exec -it 9444e1d6a76e /bin/bash
#拷贝文件到webapps

root@9444e1d6a76e:/usr/local/tomcat# cp -r ./webapps.dist/* ./webapps/
root@9444e1d6a76e:/usr/local/tomcat# cd webapps
root@9444e1d6a76e:/usr/local/tomcat/webapps# ls
docs  examples  host-manager  manager  ROOT
root@9444e1d6a76e:/usr/local/tomcat/webapps# exit
#提交一个新副本Commit a container
# docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
--author , -a		Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
--change , -c		Apply Dockerfile instruction to the created image
--message , -m		Commit message
--pause , -p	true	Pause container during commit
#仓库名小写
[root@10 ~]# docker commit -a="chen nianlai" -m="add Tomcat admin pages to webapp "  9444e1d6a76e Tomcat_cnl:1.0.0
invalid reference format: repository name must be lowercase
#提交 
[root@10 ~]# docker commit -a="chen nianlai" -m="add Tomcat admin pages to webapp "  9444e1d6a76e tomcat_cnl:1.0.0
sha256:c0a23fc2e98f28bb5722a164726921f3f2e424ae82d85565f94664e546ad434a
[root@10 ~]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED              SIZE
tomcat_cnl            1.0.0     c0a23fc2e98f   About a minute ago   481MB

docker run -p 8087:8080 tomcat_cnl:1.0.0
[root@10 ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND             CREATED          STATUS          PORTS                                                           NAMES
7a6a8e9bcb76   tomcat_cnl:1.0.0      "catalina.sh run"   8 seconds ago    Up 8 seconds    0.0.0.0:8087->8080/tcp, :::8087->8080/tcp                       eloquent_bassi

```

# Docker容器数据卷

## 1. 数据卷介绍

Docker将运用与运行的环境打包形成容器运行， Docker容器产生的数据，如果不通过docker commit生成新的镜像，使得数据做为镜像的一部分保存下来， 那么当容器删除后，数据自然也就没有了。 为了能保存数据在Docker中我们使用卷。 

数据？ 如果数据中容器中，那么容器删除，数据就丢失了？==需求： 数据可以持久化==

mysql 容器删除，但数据库需要保存。 需求： ==**数据需要保存在本地**==

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221112214951818.png" alt="image-20221112214951818" style="zoom:50%;" />

**卷就是目录或文件**，存在于一个或多个容器中，由Docker**挂载到容器**，但卷不属于联合文件系统（Union FileSystem），因此能够绕过联合文件系统提供一些用于持续存储或共享数据的特性:。

卷的设计目的就是数据的持久化，完全独立于容器的生存周期，因此Docker不会在容器删除时删除其挂载的数据卷。

数据卷的特点:

- 数据卷可在容器之间共享或重用数据
- 卷中的更改可以直接生效
- 数据卷中的更改不会包含在镜像的更新中
- 数据卷的生命周期一直持续到没有容器使用它为止
  

## 2. 使用数据卷

###  1) 运行容器，指定挂载数据卷命令：

 `docker run -it -v 主机目录:容器目录`

```bash
docker run -it -v 主机目录:容器目录

[root@10 ~]# docker run -it -v /home/test:/home centos /bin/bash
[root@a493400f86ff /]# cd home
[root@a493400f86ff home]# touch hello.java
[root@a493400f86ff home]# ls
hello.java
#宿主机
[root@10 home]# ls test
hello.java
[root@10 home]# 

```

![image-20221112215904593](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221112215904593.png)

将主机目录/home/test和容器/home建立数据卷，首先在容器目录下创建test.java文件，再去主机目录下查看是否有该文件。

![在这里插入图片描述](https://img-blog.csdnimg.cn/b81a3126d5a3422b93538d2066663ae2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQ29kZTBjZWFu,size_20,color_FFFFFF,t_70,g_se,x_16)

### 2)查看容器对应元数据

   `docker inspect 容器id`，可以在Mounts节点查看建立的数据卷信息。

```bash
[root@10 home]# docker inspect a493400f86ff

```

![image-20221112220807965](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221112220807965.png)

### 3)即使容器停止运行或者容器删除，仍然可以实现数据同步，本地的数据卷不会丢失。

![image-20221112222034948](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221112222034948.png)

## 3. MySQL容器建立数据卷同步数据

在Linux下的MySQL默认的数据文档存储目录为**/var/lib/mysql**，默认的配置文件的位置**/etc/mysql/conf.d**，为了确保MySQL镜像或容器删除后，造成的数据丢失，下面建立数据卷保存MySQL的数据和文件。



```shell
[root@10 ~]# docker run -di -p 6033:3306 -v /home/mysql/conf/my.cnf:/etc/mysql/my.cnf -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:8.0.31
945b66b307dfa4b4235c046d46939089b75537cd4277c1b510a83e6156d99435
[root@10 ~]# 
# 启动成功后进入容器
docker exec -it 71d142f5cc39 /bin/bash
mysql -p   #第1 次不需要用户名
#修改root@%用户密码
alter user 'root'@'%' identified by '123456'
#给mysql8.0增加了system_user 权限，赋权给root
grant system_user on *.* to 'root';
```

### 1） 创建新数据库

![image-20221113152042989](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221113152042989.png)

### 2) 查看卷：

![image-20221113152140053](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221113152140053.png)

### 3) 删除容器

卷仍然存在

## 4.常用卷命令

### （1）创建数据卷

```ba
docker volume create 卷名
```

### （2）查看所有的数据卷

```bash
[root@localhost data]# docker volume ls
DRIVER    VOLUME NAME
local     5bb249b15aad2c5ab7bcd61c27e26505a0f233c847989ed2154556400ce56702
local     265c41bd153c80a583d3db6408091d43b4bffda78bba58f971e903e873ff7804

```

### （3）查看指定数据卷的信息

```ba
[root@localhost data]# docker volume inspect 5bb249b15aad2c5ab7bcd61c27e26505a0f233c847989ed2154556400ce56702
[
    {
        "CreatedAt": "2022-11-13T14:22:19+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/5bb249b15aad2c5ab7bcd61c27e26505a0f233c847989ed2154556400ce56702/_data",
        "Name": "5bb249b15aad2c5ab7bcd61c27e26505a0f233c847989ed2154556400ce56702",
        "Options": null,
        "Scope": "local"
    }
]

```

### （4）删除数据卷   

`docker volume rm ...`

```bash
[root@localhost data]# docker volume rm my_vol

```

### （5）删除容器之时删除相关的卷

```bash
docker rm -v ...
```

数据卷是被设计用来持久化数据的，它的生命周期独立于容器，Docker 不会在容器被删除后自动删除数据卷，并且也不存在垃圾回收这样的机制来处理没有任何容器引用的数据卷 。如果需要在删除容器的同时移除数据卷。可以在删除容器的时候使用 docker rm -v 这个命令。

### (6) 数据卷清理

无主的数据卷可能会占据很多空间，要清理请使用以下命令

```bash
docker volume prune
```

###  (7) 使用 --mount创建数据卷

挂载一个主机目录作为数据卷。使用 --mount 标记可以指定挂载一个本地主机的目录到容器中去。

![image-20221113153753891](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221113153753891.png)

## 5. 具名挂载和匿名挂载

### 5.1匿名挂载

就是在指定数据卷的时候，**不指定容器路径对应的主机路径**。

这样对应映射的主机路径就是默认的路径**/var/lib/docker/volumes/**中自动生成一个随机命名的文件夹。

如下运行并匿名挂载Nginx容器：

```bash
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker run -d -P --name nginx01 -v /etc/nginx nginx
d3a27b969d122d5516cac75e99b17dff7aaaf1e0c042385c6b05990053f1259
```

查看所有的数据卷volume的情况, VOLUME NAME这里的值是真实存在的目录。

``` bash 
 [root@iZwz99sm8v95sckz8bd2c4Z ~]# docker volume ls
DRIVER    VOLUME NAME
local     0cd45ab893fc13971219ac5127f9c0b02491635d76d94183b0261953bdb52d26
local     668a94251e562612880a2fdb03944d67d1acdbbdae6ef7c94bee8685644f2956
local     e605f3dc4bf11ab693972592b55fb6911e5bf2083425fd58869c5f574998a09a
```

### 5.2 具名挂载

具名挂载，就是指定文件夹名称，区别于指定路径挂载，这里的指定文件夹名称是在Docker指定的默认数据卷路径下的。通过docker volume ls命令可以查看当前数据卷的目录情况。

```bash
 [root@iZwz99sm8v95sckz8bd2c4Z ~]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
4ceaff19e5275dcd3014a8e7a8af618f7f7ce0da18d605c7c41a8653e78bf912
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker volume ls
DRIVER    VOLUME NAME
local     0cd45ab893fc13971219ac5127f9c0b02491635d76d94183b0261953bdb52d26
local     668a94251e562612880a2fdb03944d67d1acdbbdae6ef7c94bee8685644f2956
local     e605f3dc4bf11ab693972592b55fb6911e5bf2083425fd58869c5f574998a09a
local     juming-nginx
```



查看指定的数据卷信息的命令：docker volume inspect数据卷名称

``` bash
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker volume inspect juming-nginx
[
    {
        "CreatedAt": "2020-12-29T22:40:25+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data",
        "Name": "juming-nginx",
        "Options": null,
        "Scope": "local"
    }
]
```

可以看到主机数据卷挂载在/var/lib/docker/volumes/juming-nginx/_data上

Docker所有的数据卷默认在/var/lib/docker/volumes/ 目录下

```bash
[root@iZwz99sm8v95sckz8bd2c4Z volumes]# ls
0cd45ab893fc13971219ac5127f9c0b02491635d76d94183b0261953bdb52d26  backingFsBlockDev                                                 juming-nginx
668a94251e562612880a2fdb03944d67d1acdbbdae6ef7c94bee8685644f2956  e605f3dc4bf11ab693972592b55fb6911e5bf2083425fd58869c5f574998a09a  metadata.db
```



匿名挂载，具名挂载，指定路径挂载的命令区别如下：
-v **容器内路径** 		#匿名挂载

-v  **卷名** : **容器内路径** 			#具名挂载

-v **/宿主机路径**:**容器内路径**	 #指定路径挂载

指定数据卷映射的相关参数：

ro —— readonly 只读。设置了只读则只能操作宿主机的路径，不能操作容器中的对应路径。

rw ----- readwrite 可读可写

```bash
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:rw nginx
```



## 6. Dockerfile中设置数据卷

我们可以在Dockerfile中使用VOLUME指令来给镜像添加一个或多个数据卷。

下面使用Dockerfile构建一个新的镜像，dockerfile01文件的内容，匿名挂载了volume01和volume02两个目录：



执行构建镜像



## 7. 容器数据卷

容器数据卷是指建立数据卷，来同步多个容器间的数据，实现容器间的数据同步。



# DockerFile构建

## 一、Dockerfile介绍（制作镜像脚本化）
### 1.1 什么是Dockerfile
Dockerfile 是一个文本文件，其内包含了一条条的指令(Instruction)，用于构建镜像。每一条指令构建一层镜像，因此每一条指令的内容，就是描述该层镜像应当如何构建。

构建步骤：

1. 编写一个dockerfile

2. docker build 构建一个镜像

3. docker run 运行 一个镜像

4. docker push 发布镜像(DockerHub, 阿里云)

   ![image-20221119224826423](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221119224826423.png)

   官方是怎么做的：

   ![image-20221119224943499](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221119224943499.png)

   官方的centos纯净版本，自己可以定制做镜像。

   ### 1.2 为什么要使用Dockerfile

   问题: 在dockerhub中官方提供很多镜像已经能满足我们的所有服务了,为什么还需要自定义镜像
   核心作用:日后用户可以将自己应用打包成镜像,这样就可以让我们应用进行容器运行.还可以对官方镜像做扩展，以打包成我们生产应用的镜像。

   完整镜像的结构图：

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/13244214e0684412aebf9d0f768e76bd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiHd3Xnmoblj6_niLE=,size_20,color_FFFFFF,t_70,g_se,x_16)

   Dockerfile的格式

   两种类型的行

   - 以# 开头的注释行
   - 由专用“指令（Instruction）”开头的指令行

   由Image Builder顺序执行各指令，从而完成Image构建。

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/8f7963ef4a064d86874f872e0a97c458.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5LiHd3Xnmoblj6_niLE=,size_20,color_FFFFFF,t_70,g_se,x_16)

   

   

   

## dockerfile构建过程

dockerfile 用于指示 docker image build 命令自动构建Image的源代码
是纯文本文件
示例：

``` bash 
docker build -f /path/Dockerfile
```

**基础知识：**

1. 每个保留关键字（指令）都必须是大写字母

2. 执行从上到下顺序执行

3. #表示注释

4. 每一个指令都会创建提交一下新的镜像层，并提交

   <img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fbbsmax.ikafan.com%2Fstatic%2FL3Byb3h5L2h0dHBzL2ltZzIwMjIuY25ibG9ncy5jb20vYmxvZy85MDk5NjgvMjAyMjAzLzkwOTk2OC0yMDIyMDMxNzE1MjYwMTMxMS0xNjEyMzUyMDgucG5n.jpg&refer=http%3A%2F%2Fbbsmax.ikafan.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1671461933&t=b4bdaf4b012e06b0c07784efa78d671c" alt="img" style="zoom:67%;" />

   dockerfile是面向开发，以后提交项目,发布就需要创建dockerfile.

   Docker镜像是企业交付的标准。

   步骤： 开发，部署，运维...

   DockerFile: 构建文件，定义了一切的步骤，源代码

   DockerImages: 通过DockerFile构建生成的镜像，最终发布和运行的产品。 

   Docker容器: 容器是镜像运行起来，提供服务

   

## 常用指令

![查看源图像](https://ts1.cn.mm.bing.net/th/id/R-C.7e554c8e9afda05e73075785c958c3b1?rik=pVLDhGgoiug4pQ&riu=http%3a%2f%2fstatic.gitlib.com%2fblog%2f2017%2f10%2f27%2fdocker2.jpg&ehk=rX%2fmx8o28le1zNp8NyBbI9liiVAlptu%2btk4SJQp84kw%3d&risl=&pid=ImgRaw&r=0&sres=1&sresct=1)





示例：　　


```bash
FROM			 指明构建的新镜像是来自于哪个基础镜像
MAINTAINER  	 指明镜像维护者及其联系方式（姓名+邮箱）
RUN				 制作镜像的时候需要运行的命令，RUN后面接linux里的命令，这个命令是在制作镜像的时候使用的
WORKDIR			 镜像的工作目录，docker exec 命令执行的时候进入的目录
VOLUME			 #向镜像创建的容器中添加数据卷，数据卷可以在容器之间共享和重用
EXPOSE			 #指明容器运行的时候暴露的端口
ONBUILD			 #当构建一个被继承DockerFile的时候，就会运行ONBUILD指令，触发指令
ENV				 #设置镜像环境变量
ADD       		 #ADD和COPY作用相似，可以从一个URL地址下载内容复制到容器的文件系统中，还可以将压缩打包格式的文件解压后复制到指定位置
COPY			 #COPY指令用来将本地的文件或者文件夹复制到镜像的指定路径下; 文件复制均使用 COPY 指令,在需要自动解压缩的场合使用 ADD 指令
CMD				 #指定这个容器启动的时候要运行的命令
				 #Dockerfile只允许使用一次CMD指令，一般都是脚本中最后一条指令
ENTRYPOINT		 #指定这个容器启动的时候要运行的命令，可以追加命令; 如果docker run后面出现与CMD指定的相同的命令，那么CMD就会被覆盖。而ENTRYPOINT会把容器名后面的所有内容都当成参数传递给其指定的命令
```



### 3.1 FROM

指定基础镜像，必须为第一个命令; 一切从这里开始

格式：
　　FROM <image>
　　FROM <image>:<tag>
　　FROM <image>@<digest>注：
   tag或digest是可选的，如果不使用这两个值时，会使用latest版本的基础镜像

### 3.2 MAINTAINER(新版即将废弃)
维护者信息

格式：
    MAINTAINER <name>
示例：

    MAINTAINER bertwu
    MAINTAINER xxx@163.com
    MAINTAINER bertwu <xxx@163.com>

 ### 3.3 RUN

构建镜像时执行的命令

### 3.4 ADD

将本地文件添加到容器中，tar类型文件会自动解压(网络压缩资源不会被解压)，可以访问网络资源，类似wget

### 3.5 COPY

功能类似ADD，但是是不会自动解压文件，也不能访问网络资源

### 3.6 CMD

构建镜像后调用，也就是在容器启动时才进行调用。

###  3.7 实战测试



Docker Hub 中 99% 的镜像都是从 scratch 这个基础镜像过来的，然后配置需要的软件和配置来进行构建。

#### 3.7.1创建一个centos镜像

```bash
FROM scratch
ADD centos-7-x86_64-docker.tar.xz /

LABEL \
    org.label-schema.schema-version="1.0" \
    org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS" \
    org.label-schema.license="GPLv2" \
    org.label-schema.build-date="20201113" \
    org.opencontainers.image.title="CentOS Base Image" \
    org.opencontainers.image.vendor="CentOS" \
    org.opencontainers.image.licenses="GPL-2.0-only" \
    org.opencontainers.image.created="2020-11-13 00:00:00+00:00"

CMD ["/bin/bash"]
```

#### 3.7.2创建tomcat镜像

准备原始包:

apache-tomcat-9.0.69.tar.gz  

jdk-8u351-linux-x64.tar.gz

```dockerfile
FROM centos:7
MAINTAINER chennl
ADD jdk-8u351-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.69.tar.gz /usr/local/
ENV JAVA_HOME /usr/local/jdk1.8.0_351
ENV CLASSPATH /usr/local/jdk1.8.0_351/lib:/usr/local/jdk1.8.0_351/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.70
ENV CATALINA_BASE /usr/local/apache-tomcat-9.0.70
ENV PATH $PATH:$JAVA_HOME/bin::$CATALINA_HOME/lib:$CATALINA_HOME/bin
 #容器运行时监听的端口
 EXPOSE
```

#### 3.7.3创建springboot镜像

参考官网: https://spring.io/guides/gs/spring-boot-docker/

- 编译springboot 应用包 xocaweb.jar, 上传到 target目录.  

  目录结构:  /u

  ```bash
  [root@master xocaweb]# pwd
  /usr/local/docker/xocaweb
  [root@master xocaweb]# ls
  docker-compose.yml  Dockerfile  target
  
  ```

  

- 下载jdk镜像：

  ```bash
  root@master ~]# docker pull openjdk:8-jdk-alpine
  ```

- 编写Dockerfile

  ```dockerfile
  FROM openjdk:8-jdk-alpine
  ARG JAR_FILE=target/*.jar
  ##创建运行用户
  RUN addgroup -S spring && adduser -S spring -G spring
  USER spring:spring
  ##复制文件
  COPY ${JAR_FILE} app.jar
  ##启动springboot
  ENTRYPOINT ["java","-jar","/app.jar"]
  ```

- 编译镜像

  ```bash
  docker build -t xocaweb:0.1.0 .
  ```

- 运行容器

  ```bash
  docker  run -p 8080:8080 xocaweb:0.1.0
  ```

- 测试：

  ```bash
  [root@master ~]# curl http://localhost:8080/hello
  <!DOCTYPE html>
  <html lang="en">>
  <head>
      <meta charset="UTF-8">
      <title>Hello</title>
  </head>
  <body>
  <h3> Hello,Spring Boot!</h3>
  2022-12-25 02:13:04
  <p>
  </p>
  
  Worker id: <div>8081</div>
  </body>
  
  ```

  

- A Better Dockerfile

  - 创建目录 target/extracted,然后解压 jar

    ```bash
    [root@master xocaweb]# mkdir target/extracted
    
    [root@master xocaweb]# java -Djarmode=layertools -jar target/*.jar extract --destination target/extracted
    
    [root@master xocaweb]# ls target/extracted
    application  dependencies  snapshot-dependencies  spring-boot-loader
    
    ```

    

  - 修改Dockerfile

    ```dock
    FROM openjdk:8-jdk-alpine
    VOLUME /tmp
    ARG EXTRACTED=target/extracted
    
    ##复制文件
    COPY ${EXTRACTED}/dependencies/ ./
    COPY ${EXTRACTED}/spring-boot-loader/ ./
    COPY ${EXTRACTED}/snapshot-dependencies/ ./
    COPY ${EXTRACTED}/application/ ./
    ##启动springboot
    ENTRYPOINT ["java","org.springframework.boot.loader.JarLauncher"]
    ```

    





> 创建一个自己的centos

```shell
FROM centos:7
MAINTAINER chennl<chennlhz@126.com>
ENV MYPATH /usr/local
WORKDIR $MYPATH
#install pakages such as  Tomcat mysql
EXPOSE 80
CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
```

builder

```bash
docker build -f mydockerfile-centos -t mycentos:0.1 .
docker images
```

![image-20221120123820475](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120123820475.png)

**docker history**查看镜像历史:

![image-20221120124017282](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120124017282.png)



## CMD和ENTRYPOINT 区别

CMD :

ETNTRYPOINT: 可以追加参数

## 实战: 制作Tomcat镜像

1. 准备压缩包

   ``` bash
    ls
   apache-tomcat-9.0.69.tar.gz    jdk-8u351-linux-x64.tar.gz
   ```

2. 编写dockerfile文件： Dockerfile 官方名字，build时不需要制定文件名

   ```Shell
   FROM centos:7
   MAINTAINER chennl<chennlhz@126.com?
   COPY readme.txt /usr/local/readme.txt
   ADD jdk-8u351-linux-x64.tar.gz /usr/local/
   ADD apache-tomcat-9.0.69.tar.gz /usr/local/
   
   ENV MYPATH /usr/local
   WORKDIR $MYPATH
   
   ENV JAVA_HOME /usr/local/jdk1.8.0_351
   ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
   ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.69
   ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.69
   ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin:$CATALINA_BASH/bin
   
   EXPOSE 8080
   
   CMD /usr/local/apache-tomcat-9.0.69/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.69/bin/logs/catalina.out
   
   
   ```

   ![image-20221120151324721](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120151324721.png)

   3. 构建镜像 docker build

      ![image-20221120152516621](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120152516621.png)

      ![](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120152600609.png)

      4. 运行容器：

      ```bash
      docker run -d -p 9090:8080 --name diytomcat -v /root/tomcat/build/webapps/test:/usr/local/apache-tomcat-9.0.69/webapps/test -v /root/tomcat/build/tomcatlogs:/usr/local/apache-tomcat-9.0.69/logs diytomcat
      ```

      ![image-20221120153330543](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120153330543.png)

      - 进入容器查看Tomcat

      ![image-20221120155140855](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120155140855.png)

      5. curl测试:

      ![image-20221120155330414](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120155330414.png)

      6. 发布项目：

         ```bash
         mkdir WEN-INFO
         cd WEB-INFO
         vim web.xml
         
         ```

          ``` xml
          <?xml version="1.0" encoding="UTF-8"?>
          <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xmlns="http://java.sun.com/xml/ns/javaee"
              xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
              xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
              id="WebApp_ID" version="2.5">
              
          </web-app>
          ```

         制作index.jsp

         ```jsp
         <%@ page language="java" contentType="text/html; charset=utf-8"
                  pageEncoding="utf-8" %>
         <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
         <html>
         <head>
             <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
             <title>首页</title>
         </head>
         <body>
         
         </body>
         </html>
         ```

         项目部署成功！

         ![image-20221120161504061](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120161504061.png)

## 发布自己的镜像

> dockerhub 账号:    username:  chennl  email: chennlhz@126.com pwd: chennl1975a

![image-20221120161827344](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120161827344.png)

- 在服务器上提交

  ``` bash
  #登录
  docker login -u chennl
  
  docker push
  ```

  ![image-20221120162024007](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120162024007.png)

  ![image-20221120162138426](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120162138426.png)

- dockerHub创建仓库

  ![image-20221120183900439](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120183900439.png)

  

- 按DockerHub 名称build
 ![image-20221120183803230](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120183803230.png)
- push到dockerhub
![image-20221120183434931](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120183434931.png)
![image-20221120183720084](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120183720084.png)

- 上传成功

  ![image-20221120184148813](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120184148813.png)

- 下载自己的镜像

  ```bash
  docker pull chennl/diytomcat:latest
  ```

  ![image-20221120184954468](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120184954468.png)

  ![image-20221120185056530](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120185056530.png)

- **修改镜像名称**

  ```bash
  docker tag [镜像ID] [修改改后镜像名]:[版本号]
  ```

> 发布到阿里云镜像服务器
>
> https://cr.console.aliyun.com/cn-hangzhou/instance/repositories

1. 登录阿里云：用户名： bcdsmstest   密码: chennl1975a

2. ```
   docker login --username=bcdsmstest registry.cn-hangzhou.aliyuncs.com
   ```

3. 创建命名空间（类似一个项目/公司）

   ![image-20221120191349803](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120191349803.png)

4. 创建镜像仓库

   ![image-20221120191851145](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221120191851145.png)

5. 操作

   ##### 1). 登录阿里云Docker Registry

   ```
   $ docker login --username=bcdsmstest registry.cn-hangzhou.aliyuncs.com
   ```

   用于登录的用户名为阿里云账号全名，密码为开通服务时设置的密码。

   您可以在访问凭证页面修改凭证密码。

   #### 2. 从Registry中拉取镜像

   ```
   $ docker pull registry.cn-hangzhou.aliyuncs.com/swirecocacola/bizmidplatform:[镜像版本号]
   ```

   ####  3. 将镜像推送到Registry

   ```
   $ docker login --username=bcdsmstest registry.cn-hangzhou.aliyuncs.com
   $ docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/swirecocacola/bizmidplatform:[镜像版本号]
   $ docker push registry.cn-hangzhou.aliyuncs.com/swirecocacola/bizmidplatform:[镜像版本号]
   ```
   #### 4. 按阿里云整个路径明编译镜像名
    ``` bash
    docker build -t  registry.cn-hangzhou.aliyuncs.com/swirecocacola/bizmidplatform:1.0 -f dockerfile-centos-cmd-entry  .
    docker push registry.cn-hangzhou.aliyuncs.com/swirecocacola/bizmidplatform:1.0
    ```
   
    ![image-20221121205623595](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121205623595.png)
   #### 5. 阿里云查看上传的像名
   
      ![image-20221121210704152](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121210704152.png)



## Docker 小结

<img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F2021021322044064.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2V5bGllcg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1671628250&t=929dc6a355bebde2be90a834d8df91e5" alt="img" style="zoom:50%;" />

``` bash
docker save [imagesname]  -o 备份文件名.tar
docker load  -i 备份文件名.tar
```

![image-20221121211623485](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121211623485.png)





## Docker 网络

### 1.理解Docker0

清空images

![image-20221121212259019](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121212259019.png)

三个网络:

> #问题: docker 如何处理网络访问的？

![image-20221121212458141](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121212458141.png)

``` bash
#启动一个容器tomcat
docker run -d -P 8080:8080 --name tomcat01 tomcat
```

![image-20221121213004663](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121213004663.png)

``` bash 
[root@localhost ~]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
33: eth0@if34: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

```

> Linux能否ping通容器内部IP?

``` bash
[root@localhost ~]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.088 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.073 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.098 ms
64 bytes from 172.17.0.2: icmp_seq=4 ttl=64 time=0.151 ms
^C
--- 172.17.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2999ms
rtt min/avg/max/mdev = 0.073/0.102/0.151/0.031 ms
#LInux可以ping容器内部
```



![image-20221121214120750](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121214120750.png)

​	如果没有ping命令:

```bash
apt update
apt install -y net-tools
apt install -y iproute2
apt install -y iputils-ping
```





``` bash
[root@localhost ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:c2:d5:51 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 80902sec preferred_lft 80902sec
    inet6 fe80::9af6:c7f3:124c:5735/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:b9:3d:75 brd ff:ff:ff:ff:ff:ff
    inet 192.168.2.84/24 brd 192.168.2.255 scope global noprefixroute dynamic enp0s8
       valid_lft 80903sec preferred_lft 80903sec
    inet6 fe80::a2cb:a887:ed75:2ea2/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:04:9e:14:fb brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:4ff:fe9e:14fb/64 scope link 
       valid_lft forever preferred_lft forever
34: vethe601a36@if33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether 32:4a:d1:6e:1a:62 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::304a:d1ff:fe6e:1a62/64 scope link 
       valid_lft forever preferred_lft forever
```



> 原理

我们每启动一个容器，Docker都会给docker容器分配一个ip, 我们只要安装了docker,就会有一个网卡docker0。桥接模式，使用的技术是evth-pair技术!

启动一个容器就多一对网卡.

![image-20221121215940696](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121215940696.png)

![image-20221121220721416](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121220721416.png)

再运行一个容器,IP增加  35,36:

![image-20221121220759393](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121220759393.png)

![image-20221121220622376](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221121220622376.png)

网卡成对出现，实现Linux与容器通讯。

可以看到容器内部和Linux主机都会创建一个新的网卡，而这两个网卡都是成对的。使用的技术就是evth-pair。evth-pair 就是一对的虚拟设备接口，他们是成对出现的，一段连着协议，一段彼此相连。evth-pair充当一个桥梁，连接各种虚拟网络设备。

**Linux主机可以直接网络连接Docker容器。Docker容器和容器之间同样可以直接网络连接的 **

tomcat01容器中ping tomcat02容器，可以看到两个容器是可以连接上的。两个Tomcat容器之间的网络交互模型图如下： 

![在这里插入图片描述](https://img-blog.csdnimg.cn/a9dff81f75834b3785d05faf474ea2a8.png)



Tomcat01和Tomcat02都使用公用的路由器docker0。所有的容器不指定网络下，都是由docker0路由的，Docker会给我们容器默认分配一个随机的可用IP地址。

容器网络互联的通用模型，如下图所示：

![在这里插入图片描述](https://img-blog.csdnimg.cn/11b689e17fcb4364ba9071bf3af14f49.png)

测试两容器之间互联:

![image-20221122144000588](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221122144000588.png)



### 2 容器互联(了解)

在微服务部署的场景下，注册中心是使用服务名来唯一识别微服务的，而我们上线部署的时候微服务对应的IP地址可能会改动，所以我们需要使用**容器名来配置容器间的网络连接**。使用–link可以完成这个功能。

首先不设置连接的情况下，是无法通过容器名来进行连接的。

![image-20221122161407746](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221122161407746.png)

下面启动一个Tomcat容器Tomcat03使用`–link` 连接已经启动的Tomcat02容器。这样容器Tomcat03就可以通过容器名Tomcat02连接到容器Tomcat02。

```bash
[root@localhost ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND             CREATED         STATUS         PORTS                                         NAMES
31cea72a6eb8   tomcat    "catalina.sh run"   6 seconds ago   Up 5 seconds   0.0.0.0:49156->8080/tcp, :::49156->8080/tcp   tomcat03
b7bc9b0d766a   tomcat    "catalina.sh run"   8 hours ago     Up 5 minutes   8080/tcp                                      tomcat02
fcb967c4306f   tomcat    "catalina.sh run"   8 hours ago     Up 2 hours     0.0.0.0:49153->8080/tcp, :::49153->8080/tcp   tomcat01
#进入tomcat03,通过名称访问tomcat02
ping tomcat02   #访问成功

```

![image-20221122163330222](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221122163330222.png)

但，在tomcat02不能通过名称访问 tomcat03

![image-20221122163524634](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221122163524634.png)



![在这里插入图片描述](https://img-blog.csdnimg.cn/e63fe1dbdacd4becb9eedd289a71c565.png)



### 3 自定义网络

因为docker0，默认情况下不能通过容器名进行访问。需要通过–link进行设置连接。这样的操作比较麻烦，更推荐的方式是自定义网络，容器都使用该自定义网络，就可以实现通过容器名来互相访问了。

#### 3.1下面查看network的相关命令

```bash
docker network --help
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/5427fb1c733f4551b6f38652f972e469.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6auY6L-H6JOd5aSp55qE5LqR,size_18,color_FFFFFF,t_70,g_se,x_16)

#### 3.2 如何自定义一个网络？

```bash
docker network create --help
```

 ![img](https://img-blog.csdnimg.cn/1f02e42a13d04dc5b9d14b54e7cbb913.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6auY6L-H6JOd5aSp55qE5LqR,size_20,color_FFFFFF,t_70,g_se,x_16)

具体使用：

```bash 
[root@haha ~]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
ed1c176a06d49da31969e229138ef5988a5fd9d2b2f2d2fc5ec1078be99a3d39
[root@haha ~]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
aa8a26ae1484   bridge    bridge    local
62cd016ed66a   host      host      local
ed1c176a06d4   mynet     bridge    local
fc650e2a675e   none      null      local
```

 

#### 3.3 查看默认的网络bridge的详细信息:

![img](https://img-blog.csdnimg.cn/5bd38f3d33cc4d37ae08c29e739f6e04.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6auY6L-H6JOd5aSp55qE5LqR,size_20,color_FFFFFF,t_70,g_se,x_16)

![](https://img-blog.csdnimg.cn/746f4babecf2463c80748c15766f6ff6.png)

查看 network create命令的相关参数

![在这里插入图片描述](https://img-blog.csdnimg.cn/7d27e6220c584e4fb36949a8a0a142d3.png)

下面自定义一个网络

```bash
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
```

参数说明：

–driver bridge 					#指定bridge驱动程序来管理网络
–subnet 192.168.0.0/16	 #指定网段的CIDR格式的子网
–gateway 192.168.0.1 		#指定主子网的IPv4或IPv6网关

   ![image-20221122164328951](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221122164328951.png)



```bash
docker network inspect mynet
```

![image-20221122164437253](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221122164437253.png)



下面启动两个容器，指定使用该自定义网络mynet，测试处于自定义网络下的容器，是否可以直接通过容器名进行网络访问。

```bash
[root@localhost ~]# docker run -d -P --name tomcat-net-01 --net mynet tomcat-net
ee0bb4f21620c93f9404a5c5de1496b2ed26197b4f2f612b9af4c0354daaa4ae
[root@localhost ~]# docker run -d -P --name tomcat-net-02 --net mynet tomcat-net
3f7e4272ddd2d8c93c200155bc693be03c59b35d6a6b57f9138f98179c02dbc5

```

![image-20221123085138633](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221123085138633.png)

下面通过容器名来测试容器tomcat-net-01 和容器tomcat-net-02之间是否能正常网络通信。

![image-20221123085748954](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221123085748954.png)

可以发现，在我们的自定义网络下，容器之间既可以通过容器名也可以通过ip地址进行网络通信。 我们自定义的网络默认已经帮我们维护了容器间的网络通信问题，这是实现网络互联的推荐方式。



### 4.Docker网络之间的互联

没有设置的情况下，不同网络间的容器是无法进行网络连接的。如图，两个不同的网络docker0和自定义网络mynet的网络模型图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/f4a3286994a4495a9dce976080eb06d3.png)

在默认网络bridge下启动容器tomcat-01,尝试连接mynet网络下的tomcat-net-01容器。
![在这里插入图片描述](https://img-blog.csdnimg.cn/5bd5baddb2ec4370bb7b04bff8904a48.png)
可以看到是无法网络连接的。

不同Docker网络之间的容器需要连接的话需要把作为调用方的容器注册一个ip到被调用方所在的网络上。需要使用`docker connect`命令。

面设置容器tomcat-01连接到mynet网络上。并查看mynet的网络详情，可以看到给容器tomcat-01分配了一个ip地址。

> docker network connect mynet tomcat-01

![image-20221123193940143](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221123193940143.png)

接下来我们就做一个网络实战联系

### 5.Docker运维

#### 5.1 查看网络

可以用这个命令查看:

```ba
[root@10 mysql-slave]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
2582fd60937a   bridge    bridge    local
8bbd1cc87a33   host      host      local
97e73063ba88   mynet     bridge    local
d1059f1d779f   none      null      local
b25ff3d97c46   redis     bridge    local

```



#### 5.2 prune命令关闭没有在使用的卷、容器、镜像

```bash
[root@10 mysql-slave]# docker volume  prune
WARNING! This will remove all local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
Deleted Volumes:
da7251cacf44b56b54f76bb297900467ff105d704aff8e52948c41fb86e1d27b
f9307e1353661b30bee929023448009d770a1e6edaf255266dcca1ef024d093c
3b2b339d89abddc6cb9ca710ffe08eacf622c7d5d837d688f3fadb4fa7ad3d07
cd6fb506a685624413c2b3fde1a515848dee28aea38db6b1318e1513d748d7e7
d0a8d8cdd68b3e79ac1e3b48826683c517b86c1ae4823fe18f9d89a29cea1f2e

Total reclaimed space: 391.8MB

```

### 6.Docker 开启远程访问

```bash
## 打开docker.service
vim /usr/lib/systemd/system/docker.service

## 在ExecStart=/usr/bin/dockerd追加
 -H unix://var/run/docker.sock  -H tcp://0.0.0.0:2375
 
 [Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
ExecReload=/bin/kill -s HUP $MAINPID
TimeoutSec=0
RestartSec=2
Restart=always

## 重新加载docker配置文件，重启Docker
root@master xocaweb]# systemctl daemon-reload
[root@master xocaweb]# systemctl restart docker

## 开防火墙端口
[root@master xocaweb]#firewall-cmd --zone=public --add-port=2375/tcp --permanent
success
[root@master xocaweb]# firewall-cmd --reload
success

```

#### 验证

http://192.168.13.131:2375/info

返回json:

```json
{"ID":"465W:A5BE:TBAW:3OHS:GE5P:IIBM:3AMT:MAPU:IQBB:ZFR6:PAK5:NMH3","Containers":1,"ContainersRunning":0,"ContainersPaused":0,"ContainersStopped":1,"Images":25,"Driver":"overlay2","DriverStatus":[["Backing Filesystem","xfs"],["Supports d_type","true"],["Native Overlay Diff","true"],["userxattr","false"]],"Plugins":{"Volume":["local"],"Network":["bridge","host","ipvlan","macvlan","null","overlay"],"Authorization":null,"Log":["awslogs","fluentd","gcplogs","gelf","journald","json-file","local","logentries","splunk","syslog"]},"MemoryLimit":true,"SwapLimit":true,"KernelMemory":true,"KernelMemoryTCP":true,"CpuCfsPeriod":true,"CpuCfsQuota":true,"CPUShares":true,"CPUSet":true,"PidsLimit":true,"IPv4Forwarding":true,"BridgeNfIptables":true,"BridgeNfIp6tables":true,"Debug":false,"NFd":27,"OomKillDisable":true,"NGoroutines":38,"SystemTime":"2022-12-25T14:04:17.159711069+08:00","LoggingDriver":"json-file","CgroupDriver":"cgroupfs","CgroupVersion":"1","NEventsListener":0,"KernelVersion":"3.10.0-1160.el7.x86_64","OperatingSystem":"CentOS Linux 7 (Core)","OSVersion":"7","OSType":"linux","Architecture":"x86_64","IndexServerAddress":"https://index.docker.io/v1/","RegistryConfig":{"AllowNondistributableArtifactsCIDRs":[],"AllowNondistributableArtifactsHostnames":[],"InsecureRegistryCIDRs":["127.0.0.0/8"],"IndexConfigs":{"docker.io":{"Name":"docker.io","Mirrors":[],"Secure":true,"Official":true}},"Mirrors":[]},"NCPU":1,"MemTotal":1019572224,"GenericResources":null,"DockerRootDir":"/var/lib/docker","HttpProxy":"","HttpsProxy":"","NoProxy":"","Name":"master","Labels":[],"ExperimentalBuild":false,"ServerVersion":"20.10.22","Runtimes":{"io.containerd.runc.v2":{"path":"runc"},"io.containerd.runtime.v1.linux":{"path":"runc"},"runc":{"path":"runc"}},"DefaultRuntime":"runc","Swarm":{"NodeID":"","NodeAddr":"","LocalNodeState":"inactive","ControlAvailable":false,"Error":"","RemoteManagers":null},"LiveRestoreEnabled":false,"Isolation":"","InitBinary":"docker-init","ContainerdCommit":{"ID":"78f51771157abb6c9ed224c22013cdf09962315d","Expected":"78f51771157abb6c9ed224c22013cdf09962315d"},"RuncCommit":{"ID":"v1.1.4-0-g5fd4c4d","Expected":"v1.1.4-0-g5fd4c4d"},"InitCommit":{"ID":"de40ad0","Expected":"de40ad0"},"SecurityOptions":["name=seccomp,profile=default"],"Warnings":["WARNING: API is accessible on http://0.0.0.0:2375 without encryption.\n         Access to the remote API is equivalent to root access on the host. Refer\n         to the 'Docker daemon attack surface' section in the documentation for\n         more information: https://docs.docker.com/go/attack-surface/"]}
```



## 实战 -Redis集群部署



![image-20221123194813656](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221123194813656.png)

## Redis 集群



``` bash
#创建网卡
docker network create redis --subnet 172.38.0.0/16

#通过脚本创建6个redis配置
for port in $(seq 1 6); \
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF > /mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done
#检查运行结果
[root@localhost ~]# cd /mydata
[root@localhost mydata]# ls
redis
[root@localhost mydata]# cd redis
[root@localhost redis]# ls
node-1  node-2  node-3  node-4  node-5  node-6
[root@localhost redis]# cd note-1 & ls
[1] 6694
node-1  node-2  node-3  node-4  node-5  node-6
[root@localhost redis]# cd node-1
[root@localhost node-1]# ls
conf
[root@localhost node-1]# cd conf
[root@localhost conf]# ls
redis.conf
[root@localhost conf]# cat redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.11
cluster-announce-port 6379
cluster-announce-bus-port 16379
[root@localhost conf]# 

#启动redis集群
docker run  -p 6371:6379  --name redis-1 \
-v /mydata/redis/node-1/data:/data \
-v /mydata/redis/node-1/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.11 redis redis-server  /etc/redis/redis.conf

docker run -p 6372:6379 --name redis-2 \
-v /mydata/redis/node-2/data:/data \
-v /mydata/redis/node-2/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.12 redis redis-server /etc/redis/redis.conf

docker run -d -p 6373:6379 --name redis-3 \
-v /mydata/redis/node-3/data:/data \
-v /mydata/redis/node-3/conf/redis.conf:/etc/redis/redis.conf \
--net redis --ip 172.38.0.13 redis redis-server /etc/redis/redis.conf

docker run -d -p 6374:6379 --name redis-4 \
-v /mydata/redis/node-4/data:/data \
-v /mydata/redis/node-4/conf/redis.conf:/etc/redis/redis.conf \
--net redis --ip 172.38.0.14 redis redis-server /etc/redis/redis.conf


#出现问题查看logs
docker logs 4d1b6cf389e9
## 1:C 27 Nov 2022 08:05:40.615 # Fatal error, can't open config file '/etc/redis/redis.conf': No such file or directory
## 分析，挂载目录写错了  修改为 :/etc/redis/redis.conf 重新启动
[root@localhost mydata]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                                       NAMES
73f045708240   redis     "docker-entrypoint.s…"   2 seconds ago        Up 1 second         0.0.0.0:6374->6379/tcp, :::6374->6379/tcp   redis-4
880cf9f32377   redis     "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:6373->6379/tcp, :::6373->6379/tcp   redis-3
11543bc4288f   redis     "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:6372->6379/tcp, :::6372->6379/tcp   redis-2
45f606fbb907   redis     "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:6371->6379/tcp, :::6371->6379/tcp   redis-1

# 进入 redis container
[root@localhost mydata]# docker exec -it redis-1 /bin/sh
# ls
appendonlydir  nodes.conf
#连接，创建集群
redis-cli --cluster create  172.38.0.11:6379 172.38.0.12:6379  172.38.0.13:6379  172.38.0.14:6379 --cluster-replicas 1

```

![image-20221129214731484](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129214731484.png)

至少要6个，那就再补充两个：

```Bash
docker run -d -p 6375:6379 --name redis-5 \
-v /mydata/redis/node-5/data:/data \
-v /mydata/redis/node-5/conf/redis.conf:/etc/redis/redis.conf \
--net redis --ip 172.38.0.15 redis redis-server /etc/redis/redis.conf

docker run -d -p 6376:6379 --name redis-6 \
-v /mydata/redis/node-6/data:/data \
-v /mydata/redis/node-6/conf/redis.conf:/etc/redis/redis.conf \
--net redis --ip 172.38.0.16 redis redis-server /etc/redis/redis.conf
```

![image-20221129215105455](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129215105455.png)

``` bash
redis-cli --cluster create  172.38.0.11:6379 172.38.0.12:6379  172.38.0.13:6379  172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
# redis-cli --cluster create  172.38.0.11:6379 172.38.0.12:6379  172.38.0.13:6379  172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.38.0.15:6379 to 172.38.0.11:6379
Adding replica 172.38.0.16:6379 to 172.38.0.12:6379
Adding replica 172.38.0.14:6379 to 172.38.0.13:6379
M: 7885f72a2ed9fb2f9db2837641db8a37a25f6119 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
M: 1bc07cdf34db1b2318c1b8d78cef2e6ab3f7ede9 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
M: e250a57394bd16b79b77679b5229e059f46e5aab 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
S: b687e5e15efc57372003f3ff89be1f7feaa4a88a 172.38.0.14:6379
   replicates e250a57394bd16b79b77679b5229e059f46e5aab
S: feeadc03c2ed1196d5313b162eec64cda627d9b6 172.38.0.15:6379
   replicates 7885f72a2ed9fb2f9db2837641db8a37a25f6119
S: 9d88a92d3889271b4d830f10365475da50d25941 172.38.0.16:6379
   replicates 1bc07cdf34db1b2318c1b8d78cef2e6ab3f7ede9
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
...
>>> Performing Cluster Check (using node 172.38.0.11:6379)
M: 7885f72a2ed9fb2f9db2837641db8a37a25f6119 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: feeadc03c2ed1196d5313b162eec64cda627d9b6 172.38.0.15:6379
   slots: (0 slots) slave
   replicates 7885f72a2ed9fb2f9db2837641db8a37a25f6119
M: e250a57394bd16b79b77679b5229e059f46e5aab 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: 9d88a92d3889271b4d830f10365475da50d25941 172.38.0.16:6379
   slots: (0 slots) slave
   replicates 1bc07cdf34db1b2318c1b8d78cef2e6ab3f7ede9
S: b687e5e15efc57372003f3ff89be1f7feaa4a88a 172.38.0.14:6379
   slots: (0 slots) slave
   replicates e250a57394bd16b79b77679b5229e059f46e5aab
M: 1bc07cdf34db1b2318c1b8d78cef2e6ab3f7ede9 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
#进入集群
redis-cli -c
127.0.0.1:6379> cluster info
127.0.0.1:6379> cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:387
cluster_stats_messages_pong_sent:396
cluster_stats_messages_sent:783
cluster_stats_messages_ping_received:391
cluster_stats_messages_pong_received:387
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:783
total_cluster_links_buffer_limit_exceeded:0
#查看集群nodes
127.0.0.1:6379> cluster nodes
feeadc03c2ed1196d5313b162eec64cda627d9b6 172.38.0.15:6379@16379 slave 7885f72a2ed9fb2f9db2837641db8a37a25f6119 0 1669538513000 1 connected
e250a57394bd16b79b77679b5229e059f46e5aab 172.38.0.13:6379@16379 master - 0 1669538512000 3 connected 10923-16383
9d88a92d3889271b4d830f10365475da50d25941 172.38.0.16:6379@16379 slave 1bc07cdf34db1b2318c1b8d78cef2e6ab3f7ede9 0 1669538513816 2 connected
7885f72a2ed9fb2f9db2837641db8a37a25f6119 172.38.0.11:6379@16379 myself,master - 0 1669538512000 1 connected 0-5460
b687e5e15efc57372003f3ff89be1f7feaa4a88a 172.38.0.14:6379@16379 slave e250a57394bd16b79b77679b5229e059f46e5aab 0 1669538511792 3 connected
1bc07cdf34db1b2318c1b8d78cef2e6ab3f7ede9 172.38.0.12:6379@16379 master - 0 1669538513000 2 connected 5461-10922
127.0.0.1:6379>
127.0.0.1:6379> set name abc
-> Redirected to slot [5798] located at 172.38.0.12:6379   # 0.12在处理
OK
172.38.0.12:6379> get name
"abc"
172.38.0.12:6379> get name
"abc"
172.38.0.12:6379> set age 17
-> Redirected to slot [741] located at 172.38.0.11:6379 #0.11在处理
OK
172.38.0.11:6379> set class one
-> Redirected to slot [7755] located at 172.38.0.12:6379 #0.12在处理
OK
172.38.0.11:6379> set k1 v1
-> Redirected to slot [12706] located at 172.38.0.13:6379 #0.13在处理
OK

```

> 如果其中一台maser出现故障，则 slave会切换成master继续工作，保持高可用







##  Docker compose

官网: https://docs.docker.com/compose/      安装: https://docs.docker.com/compose/install/

### 安装

1. 从github下载： 

   https://github.com/docker/compose/releases/download/1.29.2/docker-compose-Linux-x86_64

   ```bash 
   # 1、官网安装
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   
   ```

   

2. bash命令补全：

   ```shell
   curl -L https://raw.githubusercontent.com/docker/compose/1.29.2/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
   
   
   ```

   

3. 删除:  rm /usr/local/bin/docker-compose

4. 授权

   ```bash
   [root@docter bin]# chmod +x /usr/local/bin/docker-compose
   ```

5. 检查版本

   ```bash
   [root@docter bin]# docker-compose version
   docker-compose version 1.29.2, build 5becea4c
   docker-py version: 5.0.0
   CPython version: 3.7.10
   OpenSSL version: OpenSSL 1.1.0l  10 Sep 2019
   ```

### 使用

#### 1、相关概念

- 服务 ( service )：⼀个应⽤容器，实际上可以运⾏多个相同镜像的实例。

- 项⽬ ( project )： 由⼀组关联的应⽤容器组成的⼀个完整业务单元。 ⼀个项⽬可以由多个服务（容器）关联⽽成， **Compose ⾯向项⽬进⾏管理**。

####  2、场景
最常⻅的**项⽬**是 web ⽹站，该项⽬应该包含 web 应⽤和缓存。
1、springboot应⽤
2、mysql服务
3、redis服务
4、elasticsearch服务
.......

#### 3 docker-compose第一个案例

- 创建一个项目: mkdir xocaweb
- 在xocaweb下编写docker-cmpsoe.yml 文件

```yml
services:
   mysql:
     image: mysql
     container_name: mysql8
     ports:
       - "3306:3306"
     volumes:
        - /usr/local/mysql8.0//master/conf:/etc/mysql/conf.d
        - /usr/local/mysql8.0/master/logs:/var/log/mysql
        - /usr/local/mysql8.0/master/data:/var/lib/mysql
     environment:
        - "MYSQL_ROOT_PASSWORD=123456"

```

- 启动在xocaweb下编写docker-cmpsoe.yml 文件

  ```bash 
  docker-compose up
  ```

  

- docker-cmpsoe   build指令: 在容器期启动之前构建镜像，然后，启动docker-compose

  参考springboot进项章节 Dockerfile

  ```yaml
  services:
     mysql:
       image: mysql:latest
       container_name: mysql8
       ports:
         - "3306:3306"
       volumes:
          - /usr/local/mysql8.0//master/conf:/etc/mysql/conf.d
          - /usr/local/mysql8.0/master/logs:/var/log/mysql
          - /usr/local/mysql8.0/master/data:/var/lib/mysql
       #environment:
       #   - "MYSQL_ROOT_PASSWORD=123456"
       env_file:
           - ./mysql.env
       networks: 
  		- xocawebnet
     redis:
          image: redis:latest
          ports:
            - "6379:6379"
          networks: 
  			- xocawebnet
      xocaweb:
          build: ./  #默认当前目录下 Dockerfile文件
          ports:
          	- "8080:8080"
          #build: 
          #  context:  ./ #目录
          #  dockerfile:  Dockerfile_spring   #自己的dockerfile 文件名
          
          depends_on:
              - mysql
          networks: 
  			- xocawebnet
  	networks: 
  		xocawebnet:
  	
  ```

  

  - depends_on指令: 指定容器启动依赖

    在mysql启动之后启动，再启动 xocaweb

  - env_file指令:  类似environement

    mysql.env

    ```yaml
    MYSQL_ROOT_PASSWORD=123456
    ```

  - networks指令:  声明一个桥接网络

    在compose声明一下网络，供这个项目使用

    ```bash
    [root@master ~]# docker network ls
    NETWORK ID     NAME                 DRIVER    SCOPE
    1e81ed251fe4   bridge               bridge    local
    efbda0fb9383   host                 host      local
    2bee483b5515   none                 null      local
    c5a96c62cbbe   xocaweb_default      bridge    local
    7c475a503a51   xocaweb_xocawebnet   bridge    local
    [root@master ~]# docker inspect xocaweb_xocawebnet
    [
        {
            "Name": "xocaweb_xocawebnet",
            "Id": "7c475a503a51e567275007623d6d1d6938af89468769364c22471328f60421b1",
            "Created": "2022-12-25T14:56:27.120551878+08:00",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": null,
                "Config": [
                    {
                        "Subnet": "172.19.0.0/16",
                        "Gateway": "172.19.0.1"
                    }
                ]
            },
            "Internal": false,
            "Attachable": true,
            "Ingress": false,
    
    ```

    默认网关:

                        "Subnet": "172.19.0.0/16",
                        "Gateway": "172.19.0.1"

    容器IP:

    ```json
    "Containers": {
            "83879073e623ac528dfd149ff59eb08bc04866bf6e4ddd1dc9f6846c3ffdc494": {
                "Name": "xocaweb_redis_1",
                "EndpointID": "0b8c389243a3c053370c4e2c043ed7681b22387a9c067b1590ca1be767066374",
                "MacAddress": "02:42:ac:13:00:03",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            },
            "a1fec59fa90ec3045bbaaa350d42be02e2a6055b3d08c6b86410020c4b9ed431": {
                "Name": "mysql8",
                "EndpointID": "b5088cd1c47839198e0804fb8c7a12aafe49ea1b85442135af347f2bac95beb5",
                "MacAddress": "02:42:ac:13:00:02",
                "IPv4Address": "172.19.0.2/16",
                "IPv6Address": ""
            },
            "f7755235923801007d924efd2f155a4c6250668514848cf30ad38599667ed0a0": {
                "Name": "xocaweb_xocaweb_1",
                "EndpointID": "32a5899f76de0d1b4128773bf4404ab14be9a63b7f531fef0a794708694bc294",
                "MacAddress": "02:42:ac:13:00:04",
                "IPv4Address": "172.19.0.4/16",
                "IPv6Address": ""
            }
        },
    ```

## Portainer

Portainer是Docker的图形化管理工具，提供状态显示面板、应用模板快速部署、容器镜像网络数据卷的基本操作(包括上传下载镜像、创建容器等操作)、事件日志显示、容器控制台操作、Swarm集群和服务等集中管理和操作、登陆用户管理和控制等功能。功能十分全面，基本能满足小型单位对容器管理的全部需求。

### Portainer集群运行
- 下载Portainer镜像

  ```bash
  docker pull portainer/portainer-ce
  ```

  

- 安装Portainer(管理节点)

  ```bash
   docker run  -d -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce
  ```

- 访问验证

  http://192.168.13.131:9000/

  首先创建用户与密码: admin 123456654321

![image-20221225153525112](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221225153525112.png)







## 阿里云服务器购买：

云服务器ECS

云服务器 ECS（Elastic Compute Service）是一种安全可靠、弹性可伸缩的云计算服务，助您降低 IT 成本，提升运维效率，使您更专注于核心业务创新。

![image-20221203222634068](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203222634068.png)

![image-20221203222841785](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203222841785.png)

![](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203223058676.png)

同一个安全组：

![image-20221203223211734](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203223211734.png)

![image-20221203223303193](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203223303193.png)

![image-20221203223327174](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203223327174.png)



![image-20221203223348044](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203223348044.png)



![image-20221203223744339](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203223744339.png)

#### 4台服务器，1主4从

 

|      | Docker1       | docker2       | Docker3        | Docker4        |
| ---- | ------------- | ------------- | -------------- | -------------- |
| 公网 | 47.108.197.19 | 47.108.197.49 | 47.108.115.236 | 47.108.199.134 |
| 私网 | 172.24.82.149 | 172.24.82.150 | 172.24.82.147  | 172.24.82.148  |
|      |               |               |                |                |

  ### 4 台服务器安装Docker

和单机安装一样。 使用XShell同步安装

查看dokcer网络：



## Docker Swarm

> 购买4台服务器  2核4G

![image-20221129223750416](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129223750416.png)

> DockerSwarm工作模式

文档站点： https://docs.docker.com/engine/swarm/

# How nodes work



Docker Engine 1.12 introduces swarm mode that enables you to create a cluster of one or more Docker Engines called a swarm. A swarm consists of one or more nodes: physical or virtual machines running Docker Engine 1.12 or later in swarm mode.

There are two types of nodes: [**managers**](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#manager-nodes) and [**workers**](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#worker-nodes).  ==[**节点**]==概念- 管理节点、工作节点

![Swarm mode cluster](https://docs.docker.com/engine/swarm/images/swarm-diagram.png)

![image-20221203221208470](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203221208470.png)

## 搭建集群

![image-20221203221829255](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203221829255.png)

![image-20221203221914008](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203221914008.png)





1. 私网IP 172.24.82.149的主机成为主节点

   ```bash
   docker swarm --advertise-add 172.24.82.149
   ```
   
   
   
   ![image-20221203225405105](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221203225405105.png)
   
   ![image-20221129223923341](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129223923341.png)
   
   ```bash
   #初始化节点
   docker swarm init
   #加入一个节点
   docker swarm join
   #获取令牌
   docker swarm join-token manager
   docker swarm join-token worker
   ```

   将IP为172.24.82.150服务器做为work节点加入到 主机1中。 在机器2中执行：

   ![image-20221129224156509](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129224156509.png)

   manager 节点- 机器1中：

   ![image-20221129224246201](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129224246201.png)

   机器1中再生成1个命令，将机器3加入

   ![image-20221129224329688](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129224329688.png)

   ![image-20221129224405866](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129224405866.png)
   
   机器1中生成命令，将机器4为manager节点：
   
   ![image-20221129224450482](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129224450482.png)
   
   机器4：
   
   ![image-20221129224641229](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129224641229.png)

机器1中查看，已经有两个主节点 

![image-20221129224751271](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221129224751271.png)



## 拓展到K8s



## Linux  JAVA JAR 包

1. 配置JAVA 环境

   java 解压目录：/usr/local/jdk1.8.0_391

   ```bash
   [root@master ~]# 
   [root@master ~]# tar -zvfx jdk-8u391-linux-x64.tar.gz
   ```

   

2. 环境配置

   ```bash
   [root@master ~]# vim /etc/profile
   
   export JAVA_HOME=/usr/local/jdk1.8.0_391
   export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HONE/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
   export PATH=.:$PATH:$JAVA_HOME/bin
   
   [root@master ~]# source /etc/profile
   [root@master ~]# java -version
   [root@master ~]# javac -version
   ```

   

3. 写 HelloWorld.java

   ```bash
   [root@master ~]# vim HelloWorld.java
       
   public class HelloWorld{
      public static void main(String[] agrs){
       System.out.println("Hello,World!");
      }
   }
   [root@master ~]# javac HelloWorld.java
   [root@master ~]# java HelloWorld
   [root@master ~]# echo Main-class: HelloWorld > manifest.txt
   [root@master ~]# jar cvfm HelloWorld.jar manifest.txt HelloWorld.class
   [root@master ~]# j java -jar HelloWorld.jar
   ```

4. 制作镜像以及运行容器

   ``` bash
   做镜像[root@master ~]# vim Dockerfile
   
   FROM java:8-jdk-alpine
   MAINTAINER chennl
   COPY ./HelloWorld.jar HelloWorld.jar
   ENTRYPOINT ["java","-jar","./HelloWorld.jar"]
   
   [root@master ~]# docker build -t hello .
   [root@master hello]# docker images |grep hello
   [root@master hello]# docker run -it hello
   Hello,World!
   
   ```

   



# Linux 命令



删除文件 rm -f 文件名

删除目录  rm -rf 目录名

删除空目录  rmdir 目录名

# Linux 163镜像地址

[Index of /centos/7/os/x86_64/Packages/ (163.com)](http://mirrors.163.com/centos/7/os/x86_64/Packages/)

**JDK号分享**

```java
账号：1985479344@qq.com
密码：Oracle123
https://www.oracle.com/java/technologies/jdk-script-friendly-urls/
```

 

# alpine 

最小的linux镜像，只有5M


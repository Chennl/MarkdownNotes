# Install Docker Engine on CentOS-9

## 1.Install Docker Engine on Centos

### 1.1 Prerequisites

- CentOS 9 (stream)

### 1.2 Uninstall old version

```shell
 $ sudo dnf remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

```

> Images, containers, volumes, and networks stored in `/var/lib/docker/` aren't automatically removed when you uninstall Docker.

### 1.3 [Install using the rpm repository](https://docs.docker.com/engine/install/centos/#install-using-the-repository)

Before you install Docker Engine for first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker form the repository.

#### [Set up the repository](https://docs.docker.com/engine/install/centos/#set-up-the-repository)

Install the `dnf-plugins-core` package (which provides the commands to manage your DNF repositories) and set up the repository.

```shell
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 同步系统时间
sudo timedatectl set-ntp true
# 更新dnf证书
sudo dnf update -y ca-certificates
```

#### [Install Docker Engine](https://docs.docker.com/engine/install/centos/#install-docker-engine)

1. Install the Docker Packages.

```shell
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

2.  Start Docker Engine

```shell
    sudo systemctl enable --now docker
```

3.  daemon

   ```shell
   $ vim /etc/docker/daemon.json  
    { "registry-mirrors": ["https://docker-0.unsee.tech",
    	"https://docker-cf.registry.cyou",
    	"https://docker.1panel.live",
    	"https://wghxbdr9.mirror.aliyuncs.com"]
    	}
   $$
   [root@Tomhost ~]# systemctl daemon-reload
   [root@Tomhost ~]# systemctl restart docker
   [root@Tomhost ~]# docker info
   
   ```
   
   
   
3.  Verify that the installation is successful by running the `hello-world` image:

   ```shell
   [root@Tomhost ~]# sudo docker run hello-world
   ```
   
   

## 2.Linux Post-Installation for Docker Engine

### 1. Manage Docker as a non-root user

#### 1.1 Create the "docker" group , and add your user to the "docker" group 

```shell
$ group add docker
$ usermod -aG docker chennl
```

### 2.  Configure Docker to start on boot with systemd

```shell
 # To enabled
 $ sudo systemctl enable docker.service
 $ sudo systemctl enable containerd.service
 # to disable instead
 $ sudo systemctl enable docker.service
 $ sudo systemctl enable containerd.service
```



# Appendex

1.issue

| issue                                                        | solution                                                     |      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| chennl is not in the sudoers file.  This incident will be reported. | visudo 在这行下面添加新的一行: jack ALL=(ALL) ALL            |      |
| 1. [root@localhost ~]# docker pull hello-world<br/>Using default tag: latest<br/>Error response from daemon: Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers) | solution:<br/>	sudo mkdir -p /etc/docker<br/><br/>	sudo tee /etc/docker/daemon.json <<-'EOF'<br/>	{<br/>  	"registry-mirrors": ["https://docker-0.unsee.tech",<br/>        	"https://docker-cf.registry.cyou",<br/>        	"https://docker.1panel.live",<br/>        	"https://wghxbdr9.mirror.aliyuncs.com"]<br/>	}<br/>	EOF<br/><br/>sudo systemctl daemon-reload<br/>sudo systemctl restart docker |      |
|                                                              |                                                              |      |

##  

# Linux dnf,Yum

## 1.Yum源

#### Yum, dnf源

- centos.repo 

  配置文件是 CentOS 系统中用于配置 yum 软件仓库（repository）的文件。这个文件定义了 yum 从哪些 URL 地址获取软件包。

- centos-addons.repo 

  配置文件是 CentOS 系统中用于配置额外软件仓库（repository）的 yum 配置文件。这个文件允许用户添加除了基础仓库之外的额外软件包源，以便安装那些不在基础仓库中的软件包。



~~~shell
#1.yum 源配置
[root@localhost ~]# ls /etc/yum.repos.d/
centos-addons.repo  centos.repo
#2.备份源配置文件
[root@localhost ~]# cd /etc/yum.repos.d
[root@localhost yum.repos.d]# ls
centos-addons.repo  centos.repo
[root@localhost yum.repos.d]# cp centos.repo centos.repo.bk20250331
[root@localhost yum.repos.d]# cp centos-addons.repo centos-addons.repo.bk20250331

# 2.2 备份 DNF 的配置文件. DNF 的配置文件主要位于/etc/dnf/目录下
[root@localhost dnf]# cp dnf.conf dnf.conf.bk20250331

#3.安装 dnf-plugins-core  centos tream 9 已经默认安装
[root@localhost yum.repos.d]# dnf -y install dnf-plugins-core
Last metadata expiration check: 0:24:28 ago on Mon 31 Mar 2025 08:20:46 PM EDT.
Package dnf-plugins-core-4.3.0-20.el9.noarch is already installed.
Dependencies resolved.
Nothing to do.
Complete!
[root@localhost yum.repos.d]# dnf list installed |grep dnf-plugins
dnf-plugins-core.noarch               4.3.0-20.el9                    @anaconda
python3-dnf-plugins-core.noarch       4.3.0-20.el9                    @anaconda

# 4.config-manager使用

# 4.1 使用 --set-enabled/set-disabled 选项 启用或禁用某个软件仓库。以下是一个启用 epel 仓库的例子
[root@local dnf config-manager --set-enabled epel
[root@local dnf config-manager --set-disabled epel

# 4.2 添加新的软件仓库
#    --add-repo 选项来添加新的软件仓库。例如，添加一个自定义的仓库
[root@local]  dnf config-manager --add-repo=http://example.com/repo.repo
#     http://example.com/repo.repo 是仓库配置文件的 URL。运行该命令后，DNF 会下载这个配置文件并添加到本地的仓库列表中。

# 4.3 设置仓库优先级
# 若要为某个仓库设置优先级，可使用 --setopt 选项。比如，将 epel 仓库的优先级设为 10
# --save 选项的作用是把修改保存到配置文件里。
[root@local]  dnf config-manager --setopt=epel.priority=10 --save epel

~~~



```shell

#1.查系统是否有MySQL Yum源
$  yum repolist enabled | grep 'mysql.*-community'

#2.下载 Mysql yum源 RPM
$ sudo wget https://dev.mysql.com/get/mysql84-community-release-el9-1.noarch.rpm

#3.安装Yum源
$  sudo yum localinstall mysql84-community-release-el9-1.noarch.rpm
#4.检查安装Yum源
$ yum repolist all | grep 'mysql.*-community'

mysql-8.4-lts-community                      MySQL 8.4 LTS Community Se enabled
mysql-8.4-lts-community-debuginfo            MySQL 8.4 LTS Community Se disabled
mysql-8.4-lts-community-source               MySQL 8.4 LTS Community Se disabled
mysql-cluster-8.0-community                  MySQL Cluster 8.0 Communit disabled
mysql-cluster-8.0-community-debuginfo        MySQL Cluster 8.0 Communit disabled
mysql-cluster-8.0-community-source           MySQL Cluster 8.0 Communit disabled
mysql-cluster-8.4-lts-community              MySQL Cluster 8.4 LTS Comm disabled
mysql-cluster-8.4-lts-community-debuginfo    MySQL Cluster 8.4 LTS Comm disabled
mysql-cluster-8.4-lts-community-source       MySQL Cluster 8.4 LTS Comm disabled
mysql-cluster-innovation-community           MySQL Cluster Innovation R disabled
mysql-cluster-innovation-community-debuginfo MySQL Cluster Innovation R disabled
mysql-cluster-innovation-community-source    MySQL Cluster Innovation R disabled
mysql-connectors-community                   MySQL Connectors Community enabled
mysql-connectors-community-debuginfo         MySQL Connectors Community disabled
mysql-connectors-community-source            MySQL Connectors Community disabled
mysql-innovation-community                   MySQL Innovation Release C disabled
mysql-innovation-community-debuginfo         MySQL Innovation Release C disabled
mysql-innovation-community-source            MySQL Innovation Release C disabled
mysql-tools-8.4-lts-community                MySQL Tools 8.4 LTS Commun enabled
mysql-tools-8.4-lts-community-debuginfo      MySQL Tools 8.4 LTS Commun disabled
mysql-tools-8.4-lts-community-source         MySQL Tools 8.4 LTS Commun disabled
mysql-tools-community                        MySQL Tools Community      disabled
mysql-tools-community-debuginfo              MySQL Tools Community - De disabled
mysql-tools-community-source                 MySQL Tools Community - So disabled
mysql-tools-innovation-community             MySQL Tools Innovation Com disabled
mysql-tools-innovation-community-debuginfo   MySQL Tools Innovation Com disabled
mysql-tools-innovation-community-source      MySQL Tools Innovation Com disabled
mysql80-community                            MySQL 8.0 Community Server disabled
mysql80-community-debuginfo                  MySQL 8.0 Community Server disabled
mysql80-community-source                     MySQL 8.0 Community Server disabled


#5.选择 8.0
$ sudo yum config-manager --disable mysql-8.4-lts-community
$ sudo yum config-manager --enable mysql-8.0-community

$ yum repolist enabled| grep 'mysql*'
mysql-connectors-community           MySQL Connectors Community
mysql-tools-community                MySQL Tools Community
mysql80-community                    MySQL 8.0 Community Server



```

以上也可以通过手工修改`/etc/yum.repos.d/mysql-community.repo` file by toggling the `enabled` option. For example, a typical default entry for EL8:





## 1.Yum命令的常用操作



1. ‌**安装软件包**‌：使用 `yum install package_name` 命令来安装指定的软件包。‌
2. ‌**重新安装软件包**‌：使用 `yum reinstall package_name` 命令来重新安装软件包。‌1
3. ‌**删除软件包**‌：使用 `yum remove package_name` 命令来删除指定的软件包。
4. ‌**更新软件包**‌：使用 `yum update package_name` 命令来更新指定的软件包到最新版本。
5. ‌**列出所有可安装的软件包**‌：使用 `yum list` 命令来列出所有可安装的软件包。
6. ‌**搜索软件包**‌：使用 `yum search keyword` 命令来搜索与关键字匹配的软件包。
7. ‌**清除缓存**‌：使用 `yum clean all` 命令来清除Yum缓存。‌24
8. 查询系统上的yum源： yum repolist enabled |grep mysql.*.-community

```shell
```


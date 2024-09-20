# git

## 1.安装

~~~bash
#查看git安装版本
chennl@NHZD19002 MINGW64 ~
$ git version
git version 2.46.0.windows.1

~~~



## 2.配置

> 用户信息

```bash
#查询用户配置
chennl@NHZD19002 MINGW64 ~
$ git config --global --list
fatal: unable to read config file 'C:/Users/chennl/.gitconfig': No such file or directory

#设置用户名和邮箱
chennl@NHZD19002 MINGW64 ~
$ git config --global user.name chennl
chennl@NHZD19002 MINGW64 ~
$ git config --global user.email chennlhz@126.com
#查看配置
chennl@NHZD19002 MINGW64 ~
$ git config --global --list
user.name=chennl
user.email=chennlhz@126.com
#查看配置是否成功
chennl@NHZD19002 MINGW64 ~
$ git config -l
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=openssl
http.sslcainfo=C:/Program Files/Git/mingw64/etc/ssl/certs/ca-bundle.crt
core.autocrlf=true
core.fscache=true
core.symlinks=false
pull.rebase=false
credential.helper=manager
credential.https://dev.azure.com.usehttppath=true
init.defaultbranch=master
user.name=chennl
user.email=chennlhz@126.com

```

## 3.本地仓库

### 3.1 初始化本地仓库

```cmd
# 1. 在项目目录下（例如 D:\workspace\bmp， bmp为idea项目目录）
D:\workspace\bmp>git init
Reinitialized existing Git repository in D:/workspace/bmp/.git/
# 多了一个隐藏文件夹 .git，说明bmp目录git仓库初始化成功，即,bmp并交给git管理
2024/09/12  13:37    <DIR>          .
2024/09/12  13:37    <DIR>          ..
2024/09/15  15:59    <DIR>          .git
2024/08/18  11:16               490 .gitignore
2024/09/15  09:10    <DIR>          .idea
2024/09/03  16:10    <DIR>          common
2024/09/12  15:10    <DIR>          customer-service
2024/09/07  13:29    <DIR>          gateway-service
2024/09/07  18:52    <DIR>          message-service
2024/09/12  13:37             2,507 pom.xml
2024/09/13  10:03    <DIR>          user-service
               2 File(s)          2,997 bytes
               9 Dir(s)  15,853,481,984 bytes free
```

### 3.2 向本地仓库添加文件或目录

> git add 文件名，一般是全部文件和目录
>
> git add .

~~~ cmd
#添加全部文件与目录，所有文件进入暂存区
D:\workspace\bmp>git add .
#查看状态，绿色-代表在暂存区， 还未提交到本地仓库中
D:\workspace\bmp>git status

~~~

<img src="https://i-blog.csdnimg.cn/blog_migrate/b2a13eb5007ad0a25dd299535589972b.png" alt="img" style="zoom:67%;" />

### 3.3 提交到本地仓库

> git commit -m "提交日志"

```cmd
## 提交代码
D:\workspace\bmp>git commit -m "第1次提交代码 2024-9-15"

[master (root-commit) 662ef75] 第1次提交代码 2024-9-15
 60 files changed, 3364 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 .idea/.gitignore
 create mode 100644 .idea/dictionaries/chennl.xml
 create mode 100644 .idea/encodings.xml
 
## 查看状态
D:\workspace\bmp>git status
On branch master
nothing to commit, working tree clean  

## 查看日志
D:\workspace\bmp>git log
commit 662ef75ac6d67f35f934ac55f1042764b6c1fcb7 (HEAD -> master)
Author: chennl <chennlhz@126.com>
Date:   Sun Sep 15 16:24:12 2024 +0800

    第1次提交代码 2024-9-15
```

表示暂存区中，没有东西可以提交了



### 3.4 小结

一般情况下,我们可以这样做

- 初始化git仓库: git init

- 提交文件到暂存区: git add

- 提交暂存区的文件到本地仓库: git commit - m 提交日志

- 查看当前仓库的状态: git status

- 查看日志: git log,查看日志

## 4. 远程仓库

  ### 4.1 创建远程仓库

> https://github.com/Chennl/bmp

### 4.2  本地&远程仓库关联

~~~bash
## 1. 检查远程仓库是否关联
chennl@NHZD19002 MINGW64 /d/workspace/bmp (master)
$ git remote -v  # 返回空,说明未关联

## 2. 关联远程仓库
chennl@NHZD19002 MINGW64 /d/workspace/bmp (master)
$ git remote add origin https://github.com/Chennl/bmp.git
## 3. 检查是否关联远程仓库
chennl@NHZD19002 MINGW64 /d/workspace/bmp (master)
$ git remote -v
origin  https://github.com/Chennl/bmp.git (fetch)
origin  https://github.com/Chennl/bmp.git (push)

~~~

### 4.3 推送到远程仓库

~~~bash
chennl@NHZD19002 MINGW64 /d/workspace/bmp (master)
$ git push origin master
Enumerating objects: 21, done.
Counting objects: 100% (21/21), done.
Delta compression using up to 8 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (11/11), 903 bytes | 90.00 KiB/s, done.
Total 11 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/Chennl/bmp.git
   662ef75..b3e233b  master -> master

~~~

## 5. 提交流程

![img](git\image-20240915182713223.png)
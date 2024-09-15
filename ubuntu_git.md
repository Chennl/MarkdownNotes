## 一、安装git

## 二、进行git配置
``` root@wangjun:~# git config --global user.name 'chennlhz@126.com'``` </br>
``` root@wangjun:~# git config --global user.email 'chennlhz@126.com' ```
## 三、连接github
- 1.生成公钥

   命令行执行生成公钥命令: ```ssh-keygen -t rsa -C "chennlhz@126.com" ```
- 2. 建完公钥后，需要上传。

    使用命令cd ~/.ssh进入~/.ssh文件夹，输入gedit id_rsa.pub打开id_rsa.pub文件，复制其中所有内容。
    接着访问http://git.oschina.net/profile网页，点击SSH公钥，标题栏可以随意输入，公钥栏把你刚才复制的内容粘贴进去就OK了。
- 3. 检查是否连接成功：</br>
      输入以下命令：``` ssh -T git@github.com ```
      返回:  Hi chennl! You've successfully authenticated, but GitHub does not provide shell access.
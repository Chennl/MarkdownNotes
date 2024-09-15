Nginx

# 单体部署&集群

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221218214131998.png" alt="image-20221218214131998" style="zoom:50%;" />

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221218214314068.png" alt="image-20221218214314068" style="zoom:50%;" />

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221218214241846.png" alt="image-20221218214241846" style="zoom:50%;" />

集群：

- 高性能

- 高可靠

- 高扩展

  

# Nginx

*Nginx* (engine x) 是一个高性能的[HTTP](https://baike.baidu.com/item/HTTP?fromModule=lemma_inlink)和[反向代理](https://baike.baidu.com/item/反向代理/7793488?fromModule=lemma_inlink)web服务器 [13] ，同时也提供了IMAP/POP3/SMTP服务

## 1.Nginx安装

### 1.下载

```bash
# 1.把第三方包都放在opt目录下，所以先进入opt
[root@10 opt]# cd /opt
# 2.下载
[root@10 opt]# wget -c https://nginx.org/download/nginx-1.22.1.tar.gz
# 3.解压
[root@10 opt]# tar -zvxf nginx-1.22.1.tar.gz
[root@10 opt]# ls

```

### 2.释放，编译、安装



#### 2.1.安装依赖包**:

```bash
yum install -y gcc gcc-c++ libtool
yum install -y pcre-devel zlib zlib-devel  openssl openssl-devel

```

#### 2.2 创建makefile

https://nginx.org/en/docs/configure.html 

```bash

[root@10 opt]# ./configure \
	--prefix=/usr/local/nginx \
	--with-http_gzip_static_module 
##结果生成MakeFile文件
creating objs/Makefile
Configuration summary
  + using system PCRE library
  + OpenSSL library is not used
  + using system zlib library

  nginx path prefix: "/usr/local/nginx"
  nginx binary file: "/usr/local/nginx/sbin/nginx"
  nginx modules path: "/usr/local/nginx/modules"
  nginx configuration prefix: "/usr/local/nginx/conf"
  nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
  nginx pid file: "/usr/local/nginx/logs/nginx.pid"
  nginx error log file: "/usr/local/nginx/logs/error.log"
  nginx http access log file: "/usr/local/nginx/logs/access.log"
  nginx http client request body temporary files: "client_body_temp"
  nginx http proxy temporary files: "proxy_temp"
  nginx http fastcgi temporary files: "fastcgi_temp"
  nginx http uwsgi temporary files: "uwsgi_temp"
  nginx http scgi temporary files: "scgi_temp"
##
[root@10 opt]# ll
[root@10 nginx-1.22.1]# ls
auto  CHANGES  CHANGES.ru  conf  configure  contrib  html  LICENSE  Makefile  man  objs  README  src

```

![image-20221218224718310](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221218224718310.png)

#### 2.3 编译

```bash
[root@10 opt]# make
##编译成功
...
sed -e "s|%%PREFIX%%|/usr/local/nginx|" \
	-e "s|%%PID_PATH%%|/usr/local/nginx/logs/nginx.pid|" \
	-e "s|%%CONF_PATH%%|/usr/local/nginx/conf/nginx.conf|" \
	-e "s|%%ERROR_LOG_PATH%%|/usr/local/nginx/logs/error.log|" \
	< man/nginx.8 > objs/nginx.8
make[1]: Leaving directory `/opt/nginx-1.22.1'

```

#### 2.4 安装

```bash 
[root@10 opt]# make install
##安装成功
...
test -d '/usr/local/nginx/logs' \
	|| mkdir -p '/usr/local/nginx/logs'
make[1]: Leaving directory `/opt/nginx-1.22.1'

```

### 3. 启动

``` bash
## 1. 查看安装目录
[root@10 nginx-1.22.1]# whereis nginx
nginx: /usr/local/nginx
##2.进入sbin
[root@10 nginx-1.22.1]# cd /usr/local/nginx/sbin
##3.启动
./nginx
##4.1查看进程
[root@10 conf]# ps -ef |grep nginx
root     17457     1  0 10:08 ?        00:00:00 nginx: master process ./nginx
nobody   17458 17457  0 10:08 ?        00:00:00 nginx: worker process
root     17601  4339  0 10:10 pts/3    00:00:00 grep --color=auto nginx
##4.2 查看进程id
[root@10 conf]#  ps -C nginx -o pid
  PID
17457
17458
##4.3查看端口
netstat -anp | grep :80

```

其他命令:

```bash
[root@10 conf]# cd /opt/nginx112/sbin
[root@10 conf]# ./nginx #启动
[root@10 conf]# ./nginx -s stop #关闭
[root@10 conf]# ./nginx -s reload # 平滑重启 ，修改了nginx.conf之后，可以不重启服务，加载新的配置
[root@10 conf]# ./nginx -t  #检测配置文件是不是正确
```

### 4.验证

开端口:

``` bash
[root@10 conf]# firewall-cmd --add-port=80/tcp --permanent
success
[root@10 conf]# firewall-cmd reload

```

主机浏览器：

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221218230722269.png" alt="image-20221218230722269" style="zoom:50%;" />

## 2.配置文件

配置文件目录： /usr/local/nginx/conf/nginx.conf

```bash
[root@10 conf]# cd /usr/local/nginx/conf
[root@10 nginx]# vim nginx.conf

```

### 1. 进程

两种进程： master 进程+ work 进程

![image-20221219101327169](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221219101327169.png)

#### 1.1 work_processes

语法: worker_processes number;
默认: worker_processes 1;

```bash
[root@10 ~]# ps -ef |grep nginx
root     17457     1  0 10:08 ?        00:00:00 nginx: master process ./nginx
nobody   17458 17457  0 10:08 ?        00:00:00 nginx: worker proces
[root@10 conf]# vim nginx.conf
	worker_processes  3;
[root@10 conf]# vim ./nginx -s -reload
nginx: invalid option: "-s --reload"
[root@10 nginx]# ./sbin/nginx -s reload
[root@10 nginx]# ps -ef |grep nginx
root     17457     1  0 10:08 ?        00:00:00 nginx: master process ./nginx
nobody   23931 17457  0 12:08 ?        00:00:00 nginx: worker process
nobody   23932 17457  0 12:08 ?        00:00:00 nginx: worker process
nobody   23933 17457  0 12:08 ?        00:00:00 nginx: worker process
root     23946  4339  0 12:08 pts/3    00:00:00 grep --color=auto nginx

```

图中可以看到1个nginx主进程，master process；还有四个工作进程，worker process。主进程负责监控端口，协调工作进程的工作状态，分配工作任务，工作进程负责进行任务处理。一般这个参数要和操作系统的CPU内核数成倍数。

#### 1.2 event

![image-20221219104728673](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221219104728673.png)

```ngi
events{
	#默认使用epoll
    use epoll;
    #每个worker允许连接的客户端最大数量
    worker_connection 1024;
}
```

#### 1.4 配置文件结构

![image-20221219113750747](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221219113750747.png)

```nginx
## worker 启动的用户名；可以修改为其他用户名
#user nobody
## worker进程的数量
worker_processes 2;
#级别 debug info warn error crit
#error_log logs/error.log
#error_log logs/error.log notice
#error_log logs/error.log info

##pid文件目录
#pid logs/nginx.pid

events{
	#默认使用epoll
    use epoll;
    #每个worker允许连接的客户端最大数量
    worker_connection 1024;
}
http {
    ##导入当前目录下的mime.type文件,即 媒体类型 定义文件
    include mime.type
    default_type  application/octet-stream
	##日志格式
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    ##用户请求日志
    #access_log  logs/access.log  main;
    ##文件高效传输
    sendfile        on;
    ##数据包大到一定大小后再发送，提高传输效率
    #tcp_nopush     on;

    ##客户端口连接timeout时长,即，保持tcp连接一段时间打开
    #keepalive_timeout  0;
    keepalive_timeout  65;

​	    ##打开gzip插件, 会经过压缩传输给客户端口
    gzip  on;
    #限制最小压缩，小于1字节的文件不压缩
    gzip_min_length 1;
    #压缩级别,越大压缩比越大
    gzip_comp_level 3;
    # 压缩文件类型
   gzip_types text/plain application/x-javascript text/css image/png image/jpeg;
    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}


​	}

}
```



>  修改为 user root 运行 worker

```bash
[root@localhost nginx]# ps -ef|grep nginx
root      1935     1  0 21:39 ?        00:00:00 nginx: master process ./sbin/nginx
nobody    1936  1935  0 21:39 ?        00:00:00 nginx: worker process
[root@localhost nginx]# ./niginx -s reload
[root@localhost nginx]# ps -ef|grep nginx
root      1935     1  0 21:39 ?        00:00:00 nginx: master process ./sbin/nginx
root      2193  1935  0 21:44 ?        00:00:00 nginx: worker process

```

## location匹配规则



###  1. nginx 配置 proxy_pass 路径带 / 的问题**

用户访问的 url 为 http://klvchen.com/abc/test.html

eg:

```nginx
#情况1: proxy_pass 后带 / 
 location  /abc/ {     
         proxy_pass http://klvchen.com/;
} 
#会被代理成 http://klvchen.com/test.html
#情况2: proxy_pass 后不带 / 
location  /abc/ {
    proxy_pass http://klvchen.com;
} 
#会被代理成 http://klvchen.com/abc/test.html
```

### 2. 常见匹配

- 没有修饰符: 从指定模式开始，只支持字符串

  > location /abc{
  > root text;
  > }

下面规则都匹配:
[http://localhost/abc/sdssd](https://link.segmentfault.com/?enc=DMjLjDloZ1yK%2F0cumZxX7w%3D%3D.uDn9jdmDsv1uWX%2B%2B13Cql4qEHEBqiNJaIw3gOlVBgX0%3D)
[http://localhost/abc?page=1&s...](https://link.segmentfault.com/?enc=RmxkEONnI2sowSnH6oBsPQ%3D%3D.%2BWcfi7TJRg6lW1GCkvgYZtbzBnLDdzAzx%2Bth8%2FkFEKQM3r6sP1Qshd19eIVqHFVX)
[http://localhost/abcd](https://link.segmentfault.com/?enc=xELscwuV49PqnENrPlJKzg%3D%3D.CURPM2RgCFY0lP2q%2BPYL3WcAkFJKXICs75bKyLcuT0c%3D)
http://localhost/abc/



- ``= `` 完全匹配 URI 中除访问参数以外的内容，Linux 系统下会区分大小写，Windows 系统下则不会。

  > location = /test {
  > root text;
  > }

下面都匹配
[http://localhost/test](https://link.segmentfault.com/?enc=6hSc7mzK0VUMrZBbJJnirA%3D%3D.XPkbzxTIrkekA6R41%2BLhaTn0gBtc93HbwEPty76ugeU%3D)
[http://localhost/test?page=1&...](https://link.segmentfault.com/?enc=hMlOF3C4PtN4mH3ZAa7CHA%3D%3D.NU7LQVP5cZLU8iD4GVzFXK7ysIO2jrBO2sBCIEtm48za1zSFY7FEJTavmn%2By40Wg)
不匹配
[http://localhost/test2ds](https://link.segmentfault.com/?enc=Pmbgu2l5nBqLqUv3V3fRXQ%3D%3D.dWcmWFtA%2FKKZGp%2BqlpNNoSInWN1bk75RTX9T3mbowTI%3D)
[http://localhost/test/](https://link.segmentfault.com/?enc=SAGUWjYkTqG9zebTFUSsDg%3D%3D.pnRq4TKrngT%2BECVnGhIaXcugefBIuD1dBtmDqmLreH4%3D)

- `~` 区分大小写的正则匹配

  > location ~ /abc$ {
  > root text;
  > }

下面都匹配
[http://localhost/abc](https://link.segmentfault.com/?enc=wp3wMLnmozAUZNMi87Z8Dg%3D%3D.Ue2Jjb1b0B5bZK4oNYJqSimmnPVXWLIpF%2FKfQk1GXLI%3D)
[http://localhost/abc?p=123](https://link.segmentfault.com/?enc=uT5Hb5mklH1YR6UQzOVE6Q%3D%3D.UdukpDEm0Z9WoO8EoTPiAPYZkohBmb9qZWTfTYEtnzY%3D)
不匹配
[http://localhost/abc/](https://link.segmentfault.com/?enc=zPAeAHZLDtgbWmsCJgaxBA%3D%3D.MHkbyvvZuj4eNtjsB5V6Zn9ZMwD0vja0M3KFyCDeDl4%3D)
[http://localhost/ABC](https://link.segmentfault.com/?enc=V7clS2S8RzZGIjxGbd9HuQ%3D%3D.4aGwELguj%2FgY%2B6mZuL6ppVI%2FenbN1Icka0wh5yWG4dE%3D)
[http://localhost/abc/bbd](https://link.segmentfault.com/?enc=F%2Fj0J1JzZSftvJJo5xw4LA%3D%3D.zASEkm7lBr1NSDN7HVpb9PhVw26I4H1%2B3xDa6LjoHpg%3D)

## 3.Nginx集群

![image-20221220162408766](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221220162408766.png)

![image-20221220163506701](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221220163506701.png)

![image-20221220163715490](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221220163715490.png)

## 4.Nginx高可用 HA & Keepalived

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222153417840.png" alt="image-20221222153417840" style="zoom:67%;" />

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222153541099.png" alt="image-20221222153541099" style="zoom:67%;" /><img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222153902630.png" alt="image-20221222153902630" style="zoom:67%;" />





<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222154102305.png" alt="image-20221222154102305" style="zoom:67%;" />





## 5.LVS 负载均衡

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222163117000.png" alt="image-20221222163117000" style="zoom: 50%;" />



<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222163307907.png" alt="image-20221222163307907" style="zoom: 50%;" />



<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222163513613.png" alt="image-20221222163513613" style="zoom:50%;" />

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222163557239.png" alt="image-20221222163557239" style="zoom:50%;" />

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222165448209.png" alt="image-20221222165448209" style="zoom:50%;" />

<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222165624391.png" alt="image-20221222165624391" style="zoom:50%;" />



<img src="D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222180241183.png" alt="image-20221222180241183" style="zoom:50%;" />

# Tomcat

## Tomcat Linux主机安装

1. 检查是否安装

   ```bash
   [root@master local]# yum list |grep tomcat
   tomcat.noarch                               7.0.76-16.el7_9            updates  
   tomcat-admin-webapps.noarch                 7.0.76-16.el7_9            updates  
   tomcat-docs-webapp.noarch                   7.0.76-16.el7_9            updates  
   tomcat-el-2.2-api.noarch                    7.0.76-16.el7_9            updates  
   tomcat-javadoc.noarch                       7.0.76-16.el7_9            updates  
   tomcat-jsp-2.2-api.noarch                   7.0.76-16.el7_9            updates  
   tomcat-jsvc.noarch                          7.0.76-16.el7_9            updates  
   tomcat-lib.noarch                           7.0.76-16.el7_9            updates  
   tomcat-servlet-3.0-api.noarch               7.0.76-16.el7_9            updates  
   tomcat-webapps.noarch                       7.0.76-16.el7_9            updates  
   tomcatjss.noarch                            7.2.5-1.el7                base 
   
   ```

   

2. 下载并解压

    https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.70/bin/apache-tomcat-9.0.70.tar.gz

   ```bash 
   [root@master local]# pwd
   /usr/local
   [root@master local]# tar -vxf apache-tomcat-9.0.70.tar.gz
   #创建软链接
   [root@master local]# ln -s /usr/local/apache-tomcat-9.0.70 /usr/local/tomcat9.0
   
   ```

3. 检查版本

   ```bash 
   [root@master tomcat9.0]# ./bin/version.sh
   Using CATALINA_BASE:   /usr/local/tomcat9.0
   Using CATALINA_HOME:   /usr/local/tomcat9.0
   Using CATALINA_TMPDIR: /usr/local/tomcat9.0/temp
   Using JRE_HOME:        /usr/local/jdk1.8.0_351
   Using CLASSPATH:       /usr/local/tomcat9.0/bin/bootstrap.jar:/usr/local/tomcat9.0/bin/tomcat-juli.jar
   Using CATALINA_OPTS:   
   Server version: Apache Tomcat/9.0.70
   Server built:   Dec 1 2022 14:05:47 UTC
   Server number:  9.0.70.0
   OS Name:        Linux
   OS Version:     3.10.0-1160.el7.x86_64
   Architecture:   amd64
   JVM Version:    1.8.0_351-b10
   JVM Vendor:     Oracle Corporation
   
   ```

   4.启动的关闭

   ```bash 
   #启动
   [root@master tomcat9.0]# ./bin/startup
   -bash: ./bin/startup: No such file or directory
   [root@master tomcat9.0]# ./bin/startup.sh
   Using CATALINA_BASE:   /usr/local/tomcat9.0
   Using CATALINA_HOME:   /usr/local/tomcat9.0
   Using CATALINA_TMPDIR: /usr/local/tomcat9.0/temp
   Using JRE_HOME:        /usr/local/jdk1.8.0_351
   Using CLASSPATH:       /usr/local/tomcat9.0/bin/bootstrap.jar:/usr/local/tomcat9.0/bin/tomcat-juli.jar
   Using CATALINA_OPTS:   
   Tomcat started.
   # 检查进程:
   [root@master tomcat9.0]# ps -ef|grep java
   root      48191      1  3 03:50 pts/0    00:00:02 /usr/local/jdk1.8.0_351/bin/java -Djava.util.logging.config.file=/usr/local/tomcat9.0/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources -Dorg.apache.catalina.security.SecurityListener.UMASK=0027 -Dignore.endorsed.dirs= -classpath /usr/local/tomcat9.0/bin/bootstrap.jar:/usr/local/tomcat9.0/bin/tomcat-juli.jar -Dcatalina.base=/usr/local/tomcat9.0 -Dcatalina.home=/usr/local/tomcat9.0 -Djava.io.tmpdir=/usr/local/tomcat9.0/temp org.apache.catalina.startup.Bootstrap start
   root      48283  45138  0 03:51 pts/0    00:00:00 grep --color=auto java
   # 检查端口
   [root@master tomcat9.0]# ss -lntup |grep java
   tcp    LISTEN     0      1        [::ffff:127.0.0.1]:8005               [::]:*                   users:(("java",pid=48191,fd=66))
   tcp    LISTEN     0      100    [::]:8080               [::]:*                   users:(("java",pid=48191,fd=57))
   #关闭
   [root@master tomcat9.0]# ./bin/shutdown.sh
   Using CATALINA_BASE:   /usr/local/tomcat9.0
   Using CATALINA_HOME:   /usr/local/tomcat9.0
   Using CATALINA_TMPDIR: /usr/local/tomcat9.0/temp
   Using JRE_HOME:        /usr/local/jdk1.8.0_351
   Using CLASSPATH:       /usr/local/tomcat9.0/bin/bootstrap.jar:/usr/local/tomcat9.0/bin/tomcat-juli.jar
   Using CATALINA_OPTS: 
   ```

   5. 开防火墙端口 8080

      ```bash 
      [root@master tomcat9.0]# firewall-cmd --add-port=8080/tcp --permanent
      success
      [root@master tomcat9.0]# firewall-cmd --reload
      success
      ```

   6. 浏览器测试 成功

   ## 文件

1. conf

   - server.xml 配置文件

     ```bash
     #关闭tomcat的端口
     <Server port="8005" shutdown="SHUTDOWN">
       <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
       
     
     #用户请求进入connecter ,然后交给engine处理
     <Connector port="8080" protocol="HTTP/1.1"
                    connectionTimeout="20000"
                    redirectPort="8443" />
     #交给engine处理，默认主机=localhost
     # host tomcat中的虚拟主机
     <Engine name="Catalina" defaultHost="localhost">
     
     # name 为网站域名
     # appBase 站点文件目录
     # value 日志配置
           <Host name="localhost"  appBase="webapps"
                 unpackWARs="true" autoDeploy="true">
     
             <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
                    prefix="localhost_access_log" suffix=".txt"
                    pattern="%h %l %u %t &quot;%r&quot; %s %b" />
           </Host>
         </Engine>
     ```

     

   - tomcat-users.xml

     ```xml
       <!-- 激活角色，修改用户密码-->
       <role rolename="manager-gui"/>
       <role rolename="admin-gui"/>
       <user username="tomcat" password="tomcat" roles="manager-gui, admin-gui"/>
     
     ```

   - 验证：

     ```bash
     [root@master tomcat9.0]# curl -utomcat:tomcat http://127.0.0.1:8080/manager/html
     #8.5以后管理页面限制只能 127.0.0.1地址才能访问
     #修改 /usr/local/apache-tomcat-9.0.70/webapps/manager/META-INF/tomcat-users.xml
      <Valve className="org.apache.catalina.valves.RemoteAddrValve"
              allow="\d+\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
     ```

     

5. logs:

   - catalina.out: 主日志
   - localhost_access_log.2022-12-21.txt 访问日志

   - Tomcat的启动日志输出在Tomcat的安装目录下的logs目录中，Tomcat的启动及运行日志文件名为 catalina.out，所以我们查看Tomcat启动日志，主要可以通过两条指令，如下：

     ```bash
     #1. 分页查询Tomcat的日志信息
        more /usr/local/apache-tomcat-7.0.57/logs/catalina.out
     
     #2. 查询日志文件尾部的50行记录
        tail -50 /usr/local/apache-tomcat-7.0.57/logs/catalina.out
     ```

     

   

## Tomcat Docker

## Nginx集群 -  Springboot App

![image-20221222125642430](D:\SwirebevUser\chennl\AppData\Roaming\Typora\typora-user-images\image-20221222125642430.png)

1. 发布3个相同的jar、端口分别是8081、8082、8083

   ```yaml
   server:
     port: 8083
   worker:
     id: 8083
     version: 1.0.0
   ```

2. Linux后台运行

   ```bash
   #后台运行
   [root@master springboot]# nohup java -jar xocaweb-8081.jar & >xoc8081.log&
   [root@master springboot]# nohup java -jar xocaweb-8082.jar & >xoc8082.log&
   [root@master springboot]# nohup java -jar xocaweb-8083.jar & >xoc8083.log&
   
   #查看进程
   [root@master springboot]# ps -ef |grep java
   root      72829  70784  0 22:55 pts/1    00:00:13 java -jar xocaweb-8081.jar
   root      75297  70784 18 23:42 pts/1    00:00:06 java -jar xocaweb-8082.jar
   root      75318  70784  9 23:42 pts/1    00:00:01 java -jar xocaweb-8083.jar
   root      75343  70784  0 23:42 pts/1    00:00:00 grep --color=auto java
   
   #查看端口
   [root@master springboot]# ss -tnlup |grep 808
   tcp    LISTEN     0      100    [::]:8081               [::]:*                   users:(("java",pid=72829,fd=17))
   tcp    LISTEN     0      100    [::]:8082               [::]:*                   users:(("java",pid=75297,fd=17))
   tcp    LISTEN     0      100    [::]:8083               [::]:*                   users:(("java",pid=75318,fd=17))
   
   ```

   

3. Nginx负载均衡配置

   nginx内置负载均衡策略主要分为三大类，分别是轮询、最少连接和ip hash

   - round-robin 轮询1:1轮流处理请求(默认)
     每个请求按时间顺序逐一分配到不同的应用服务器，如果应用服务器down掉，自动剔除，剩下的继续轮询。
   - weight 权重(加权轮询)
     通过配置权重，指定轮询几率，权重和访问比率成正比，用于应用服务器性能不均的情况。
   - ip_hash 哈希算法
     每个请求按访问ip的hash结果分配，这样每个访客固定访问一个应用服务器，可以解决session共享的问题。应用服务器如果故障需要手工down掉。

4. 

   ```nginx
   
   worker_processes  1;
   
   events {
       worker_connections  1024;
   }
   http{
   	upstream www.cluster.servers{
   					server localhost:8081;
   					server localhost:8082;
   					server localhost:8083;
   
   	}
   
       server {
           listen       80;
           server_name  localhost;
   
           #charset koi8-r;
   
           #access_log  logs/host.access.log  main;
   
   		location / {
   			proxy_pass  http://www.cluster.servers; 
   		}
   
       }
   
   
   }
   ```

   

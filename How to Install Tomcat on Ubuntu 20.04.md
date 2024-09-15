[How to Install Tomcat on Ubuntu20.04](https://www.hostinger.com/tutorials/how-to-install-tomcat-on-ubuntu/)
## Step 1: Install Java
## Step 2: Create Tomcat User
- Create a new tomcat group that will run the service: ``` sudo groupadd tomcat```
- Create user members of the Tomcat group with a home directory opt/tomcat for running the Tomcat service: ```sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat ```
- Step 3: Install Tomcat on Ubuntu
The best way to install Tomcat 9 on Ubuntu is to download the latest binary release from the Tomcat 9 downloads page and configure it manually. If the version is not 9.0.17 or it’s the latest version, then follow the latest stable version. Just copy the link of the core tar.gz file under the Binary Distributions section.

Now, change to the /tmp directory on your server to download the items which you won’t need after extracting the Tomcat contents:
   + ``` cd /tmp ``
   + ``` curl -O https://www-us.apache.org/dist/tomcat/tomcat-9/v9.0.40/bin/apache-tomcat-9.0.40.tar.gz```
## Step 4: Update Permissions
  + sudo mkdir /opt/tomcat
  + cd /opt/tomcat
  + sudo tar xzvf /tmp/apache-tomcat-9.0.*tar.gz -C /opt/tomcat --strip-components=1
      Now, give the Tomcat group ownership over the entire installation directory with the chgrp command:
      ``` sudo chgrp -R tomcat /opt/tomcat ```
      ``` sudo chmod -R g+r conf ```
      ``` sudo chmod g+x conf ```
      ``` sudo chown -R tomcat webapps/ work/ temp/ logs/ bin/ ```
## Step5: Create a systemd Unit File
  + ``` sudo nano /etc/systemd/system/tomcat.service ```
  +  ``` [Unit]
        Description=Apache Tomcat Web Application Container
        After=network.target
    
        [Service]
        Type=forking
    
        Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64/jre
        Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
        Environment=CATALINA_Home=/opt/tomcat
        Environment=CATALINA_BASE=/opt/tomcat
        Environment=’CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC’
        Environment=’JAVA_OPTS.awt.headless=true -Djava.security.egd=file:/dev/v/urandom’
    
        ExecStart=/opt/tomcat/bin/startup.sh
        ExecStop=/opt/tomcat/bin/shutdown.sh
    
        User=tomcat
        Group=tomcat
        UMask=0007
        RestartSec=10
        Restart=always
    
        [Install]
    
        WantedBy=multi-user.target 
    ```
+ ``` sudo systemctl daemon-reload ```
+ ``` cd /opt/tomcat/bin ```
+ ``` sudo ./startup.sh run ```
## Step6: Adjust the Firewall
 + ```sudo ufw allow 8080 ``` 

## Step 7: Configure the Tomcat Web Management Interface
+ Follow the command below to add a login to your Tomcat user and edit the tomcat-users.xml file:
``` sudo nano /opt/tomcat/conf/tomcat-users.xml ```
 - xxxxxxxxxx96 1// An highlighted block2var foo = 'bar';3(```)  // 注：为了防止转译，实际中去掉两边小括号即可!4​5表格6---------------------------7项目     | Value8-------- | -----9电脑  | $160010手机  | $1211导管  | $112​13| Column 1 | Column 2      |14|:--------:| -------------:|15| centered 文本居中 | right-aligned 文本居右 |16​17自定义列表18---------------------------19Markdown20:  Text-to-HTML conversion tool21​22Authors23:  John24:  Luke25​26注脚27---------------------------28一个具有注脚的文本。[^1]29​30[^1]: 注脚的解释31​32注释33---------------------------34Markdown将文本转换为 HTML。35​36*[HTML]:   超文本标记语言37​38LaTeX 数学公式39---------------------------40Gamma公式展示 $\Gamma(n) = (n-1)!\quad\forall41n\in\mathbb N$ 是通过 Euler integral42​43$$44\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.45$$46​47插入甘特图48---------------------------49``` mermaid50gantt51        dateFormat  YYYY-MM-DD52        title Adding GANTT diagram functionality to mermaid53        section 现有任务54        已完成               :done,    des1, 2014-01-06,2014-01-0855        进行中               :active,  des2, 2014-01-09, 3d56        计划中               :         des3, after des2, 5d57``` // 注：为了防止转译，实际中去掉两边小括号即可!58​59​60插入UML图61------------62``` mermaid63sequenceDiagram64张三 -->>  李四: 你好！李四, 最近怎么样?65李四-->>王五: 你最近怎么样，王五？66李四--x 张三: 我很好，谢谢!67李四-x 王五: 我很好，谢谢!68Note right of 王五: 李四想了很长时间, 文字太长了<br/>不适合放在一行.69​70李四-->>张三: 打量着王五...71张三->>王五: 很好... 王五, 你怎么样?72``` // 注：为了防止转译，实际中去掉两边小括号即可!73​74插入Mermaid流程图75--------76```mermaid77graph LR78A[长方形] -- 链接 --> B((圆))79A --> C(圆角长方形)80B --> D{菱形}81C --> D82 ```  // 注：为了防止转译，实际中去掉两边小括号即可!83​84插入Flowchart流程图85-------86```mermaid87flowchat88st=>start: 开始89e=>end: 结束90op=>operation: 我的操作91cond=>condition: 确认？92​93st->op->cond94cond(yes)->e95cond(no)->op96 ```  // 注：为了防止转译，实际中去掉两边小括号即可!javascript
 ```
 tomcat-users.xml — Admin User

<role rolename="manager-gui" />
<role rolename="admin-gui" />
<user username="admin" password="password" roles="manager-gui,admin-gui"/>

</tomcat-users>
 ```
+ For the Manager app, type:
``` sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml ```
+ For the Host Manager app, type:
``` sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml ```
+ To restart the Tomcat service and view the effects:
``` sudo systemctl restart tomcat```


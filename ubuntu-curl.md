curl命令是个功能强大的网络工具，支持通过http、ftp等方式下载文件、上传文件。还可以用来抓取网页、网络监控等方面的开发，解决开发过程中遇到的问题。
###  ``` 常用参数 ```
curl命令参数很多，这里只列出我曾经用过、特别是在shell脚本中用到过的那些。
-v/--verbose 小写的v参数，用于打印更多信息，包括发送的请求信息，这在调试脚本是特别有用。
-m/--max-time <seconds> 指定处理的最大时长
-H/--header <header> 指定请求头参数
-s/--slient 减少输出的信息，比如进度
--connect-timeout <seconds> 指定尝试连接的最大时长
-x/--proxy <proxyhost[:port]> 指定代理服务器地址和端口，端口默认为1080
-T/--upload-file <file> 指定上传文件路径
-o/--output <file> 指定输出文件名称
-d/--data/--data-ascii <data> 指定POST的内容
--retry <num> 指定重试次数
-e/--referer <URL> 指定引用地址
-I/--head 仅返回头部信息，使用HEAD请求



### 1、curl安装
eg、sudo apt-get install curl


### 2、GET请求
``` curl http://www.baidu.com，回车之后，HTML内容打印在屏幕上；如果这里的URL指向的是一个文件或者一幅图都可以直接下载到本地。
curl -i "http://www.baidu.com"  显示全部信息
curl -l "http://www.baidu.com" 只显示头部信息
curl -v "http://www.baidu.com" 显示get请求全过程解析
```

wget "http://www.baidu.com"也可以


### 3、下载
``` curl –o linjiqin http://www.cnblogs.com/linjiqin ```，执行后可以看到下载进度提示，完成100%后会自动退出了，把网页保存到linjiqin中。

它还有一个大写O的选项，是按照服务器上的文件名保存到本地，如果执行curl –O http://www.cnblogs.com，是会报错的，提示找不到文件名，如果换成curl –O http://www.cnblogs.com/linjiqin/p/5401969.html，就自动保存文件为5401969.html。


### 4、上传
-T/--upload-file：往服务器上传文件

用法：
上传多个文件
curl -T "img[1-1000].png" ftp://example.com/upload/

上传多个文件
curl -T "{file1,file2}" http://www.example.com


### 5、POST方法
-d或--data参数：post请求，用法为curl -d "id=1&name=test" http://example.com/example.php，需把请求的参数和URL分开，同时可以使用curl -d "id=1" -d "name=test" http://example.com/example.php，相当于提交了两个参数。当提交的参数值中有特殊字符就需要先转义。如空格时，就需要转义成%20。

--data-urlencode参数：可以自动转义成特殊字符，无需人工事先转义。
curl --data-urlencode "name=April 1" http://example.com/example.php

-F或--form：将本地文件上传到服务器，用法为：curl -F "filename=@/home/test/test.pic" http://example.com/example.php 。千万不能漏掉@符号。


6、设置referer
有时候我们如果直接请求某个URL不能成功，它需要判断referer是否正确，那就可以通过-e或--referer参数模拟
curl --referer http://www.example.com http://www.example.com


7、指定User Agent
-A/--user-agent：伪装成指定的浏览器Chrome访问，用法：

curl -A "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/31.0.1650.63 Safari/537.36" www.baidu.com


8、伪造cookie
-b或--cookie: 有两种用法，一是指定参数和值：curl --cookie "name=xxx" http://www.example.com ；二是从文件读取：curl -b /cookie.txt http://www.example.com


9、保存cookie
-c/--cookie-jar：curl命令执行后保存操作时生成的cookie到文件：

curl -c ./cookie.txt -d username=aaaa -d pwd=****** http://www.example.com 


10、定义输出显示内容
-w/--write-out: 可以定义输出的内容，如常用的http码，tcp连接时间，域名解析的时间，握手时间及第一时间响应时间等，非常强大。

1、打印出返回的http码
curl -o /dev/null -s -w %{http_code} "http://www.baidu.com" 
2、打印响应时间
curl -o /dev/null -s -w "time_total: %{time_total}\n" "http://www.baidu.com" 
# Python+VMware + Docker + VS Code 远程开发



### **Step 1: 创建 Python 开发容器**

创建一个基于官方 Python 镜像的开发容器，包含常用工具：

```sh
# 创建项目目录
mkdir python-project && cd python-project

# 创建Dockerfile
vim Dockerfile 
FROM python:3.12-slim
RUN apt-get update 
RUN apt-get install -y git curl && rm -rf /var/lib/apt/lists
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
EXPOSE 8000
CMD ["python", "app.py"]


# 创建requirements.txt示例
vim requirements.txt  
blinker==1.9.0
certifi==2025.6.15
charset-normalizer==3.4.2
click==8.2.1
et_xmlfile==2.0.0
Flask==3.1.1
idna==3.10
itsdangerous==2.2.0
Jinja2==3.1.6
MarkupSafe==3.0.2
numpy==2.3.1
openpyxl==3.1.5
requests==2.32.4
urllib3==2.5.0
Werkzeug==3.1.3


# 构建Docker镜像
docker build -t python-dev .

# 运行容器并挂载当前目录
docker run -itd \
  --name python-container \
  -v $(pwd):/app \
  -p 8000:8000 \
  python-dev bash
```

#### **Step 2: 配置 VS Code 远程开发**

1. **在 Windows 上安装 VS Code**，并安装以下扩展：

   - **Remote - SSH**：连接到虚拟机。
   - **Python**：提供代码补全、调试等功能。

2. **配置 SSH 连接**：

   - 打开 VS Code，按`F1`输入`Remote-SSH: Add New SSH Host`。
   - 输入连接命令：`ssh username@192.168.1.100`（虚拟机 IP）。
   - 选择密钥文件（或使用密码认证）。

3. **连接到虚拟机**：

   - 按`F1`输入`Remote-SSH: Connect to Host`，选择刚添加的虚拟机。

   - 在远程窗口中打开终端，验证能否访问 Docker：

     ```sh
     docker ps  # 应显示正在运行的容器
     
     ```

#### **Step 3: 在容器中开发**

3.1 **打开项目目录**：

- 在容器终端中，导航到挂载的项目目录：

- ```sh
  cd /app
  ```

- 对应主机项目目录  python-project：

```bash
 # python-project
 [root@Tomhost python-project]# vim app.py
 from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello, Docker + VS Code!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8000, debug=True)


```

- 在Docker 中运行：

```bash
root@9dfce0e1dbfd:/app# python ./app.py
 * Serving Flask app 'app'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:8000
 * Running on http://172.17.0.2:8000
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 472-193-210
172.17.0.1 - - [05/Jul/2025 10:43:10] "GET / HTTP/1.1" 200 -

```

- 在主机项目目录：

  ```bash
  
  [root@Tomhost python-project]# curl 127.0.0.1:8000
  Hello, Docker + VS Code![
  ```

  

3.2**在 VS Code 中打开容器**： 

- 按`F1`输入`Dev Containers: Attach to Running Container`。
- 选择`python-container`。
- 选择 打开目录 /app/
- 
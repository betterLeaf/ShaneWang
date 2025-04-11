---
title: 02-部署Vue3项目
date: 2024-12-09 14:15:09

tags:  
    - Nginx
cover: https://i.ibb.co/CBD8r9L/a99e7ab8-2133-4451-ba76-45e952032de0.webp
categories: 容器&部署
---



### 一、安装与配置Nginx
---
#### 1. **下载并安装Nginx**
- **操作步骤**：
  1. 访问[Nginx官网](http://nginx.org/en/download.html)，选择 **Windows 1.25.3** 版本（当前最新稳定版）下载。
  2. 将下载的压缩包解压到 `E:\soft\nginx-1.25.3`（解压后目录结构应包含 `conf`、`html`、`logs` 等文件夹）。

#### 2. **放置Vue3的dist包**
- **操作步骤**：
  1. 将Vue3打包生成的 `dist` 文件夹复制到 `E:\soft\nginx-1.25.3\html\dist`。
  2. 确保 `dist` 目录中包含 `index.html` 和其他静态资源文件。

#### 3. **配置Nginx代理**
- **操作步骤**：
  1. 打开配置文件：`E:\soft\nginx-1.25.3\conf\nginx.conf`。
  2. 修改以下内容：
     ```nginx
     server {
         listen       8848;       # 监听端口
         server_name  localhost;  # 服务器名（可自定义域名）

         # 静态资源服务
         location / {
             root   html/dist;    # 指向dist目录
             index  index.html index.htm;
             try_files $uri $uri/ /index.html;  # 处理Vue路由
         }

         # 反向代理API请求
         location /api {
             proxy_pass http://www.wsjtest.com;  # 代理到目标接口地址
             proxy_set_header Host $host;
             proxy_set_header X-Real-IP $remote_addr;
             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
             # 若接口需重写路径，添加以下规则：
             # rewrite ^/api/(.*)$ /$1 break;
         }
     }
     ```

### 二、启动与验证
---
#### 1. **启动Nginx**
- **操作步骤**：
  1. 打开命令行，进入Nginx目录：`cd E:\soft\nginx-1.25.3`。
  2. 输入命令启动服务：`start nginx`（无报错即启动成功）。
  3. 查看进程是否运行：`tasklist /fi "imagename eq nginx.exe"`。

#### 2. **验证访问**
- **操作步骤**：
  1. 浏览器访问 `http://localhost:8848`，若显示Vue项目页面，说明静态资源服务配置成功。
  2. 检查API代理是否生效：在前端代码中发起 `/api/xxx` 请求，观察是否代理到 `www.wsjtest.com`。

### 三、常见问题处理
---
#### 1. **端口冲突**
- **解决方案**：
  - 检查8848端口占用：`netstat -ano | findstr :8848`。
  - 若端口被占用，修改 `nginx.conf` 中的 `listen` 端口值并重启。

#### 2. **配置文件错误**
- **验证配置语法**：  
  ```bash
  nginx -t -c E:\soft\nginx-1.25.3\conf\nginx.conf
  ```
  - 输出 `test is successful` 表示配置正确。

#### 3. **重载配置**
- **无需重启服务**：  
  ```bash
  nginx -s reload
  ```

### 四、可选：设置开机自启动
---
1. **使用WinSW工具**：
   - 下载 [WinSW](https://github.com/winsw/winsw/releases)，将 `winsw.exe` 重命名为 `nginx-service.exe`，放入 `E:\soft\nginx-1.25.3`。
   - 创建配置文件 `nginx-service.xml`：
     ```xml
     <service>
         <id>nginx</id>
         <name>nginx</name>
         <executable>E:\soft\nginx-1.25.3\nginx.exe</executable>
         <stopexecutable>E:\soft\nginx-1.25.3\nginx.exe -s stop</stopexecutable>
     </service>
     ```
   - 安装服务：  
     ```bash
     nginx-service.exe install
     ```

---

### 最终文件路径汇总
| 文件/目录                 | 路径                                  |
|--------------------------|---------------------------------------|
| Nginx安装目录            | `E:\soft\nginx-1.25.3`                |
| Vue3的dist目录           | `E:\soft\nginx-1.25.3\html\dist`     |
| 主配置文件               | `E:\soft\nginx-1.25.3\conf\nginx.conf` |
| WinSW配置文件           | `E:\soft\nginx-1.25.3\nginx-service.xml` |

通过以上步骤，你的Vue3项目将通过Nginx代理在8848端口运行，并自动转发API请求到目标地址。
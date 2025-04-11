---
title: 03-多项目配置
date: 2024-12-09 14:15:09

tags:  
    - Nginx
cover: https://i.ibb.co/CBD8r9L/a99e7ab8-2133-4451-ba76-45e952032de0.webp
categories: 容器&部署
---


---

#### **1. 创建子配置文件目录**
- **路径**：`E:\soft\nginx-1.25.3\conf\conf.d`
  - 在Nginx安装目录的`conf`下新建`conf.d`文件夹，用于存放子配置文件（参考网页1、8、22）。
  - *作用*：避免主配置臃肿，便于维护多个项目配置。

#### **2. 新建子配置文件**
- **文件路径**：`E:\soft\nginx-1.25.3\conf\conf.d\vue-proxy.conf`
  - 文件内容如下（代理配置核心）：
    ```nginx
    server {
        listen       8848;          # 监听端口
        server_name  localhost;      # 服务器名或自定义域名

        # 静态资源服务（指向Vue的dist目录）
        location / {
            root   html/dist;       # 静态文件路径
            index  index.html;
            try_files $uri $uri/ /index.html;  # 处理Vue路由
        }

        # 反向代理（移除/api前缀）
        location /api {
            rewrite ^/api(/.*)$ $1 break;       # 移除/api前缀
            proxy_pass http://www.wsjtest.com;  # 目标接口地址
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
    ```
  - *关键点*：  
    - `rewrite`规则将`/api/xxx`重写为`/xxx`，实现路径透明化（参考网页25中的反向代理配置）。
    - 子配置文件命名建议带项目名，如`vue-proxy.conf`，便于识别（参考网页6、8）。

#### **3. 在主配置中引入子文件**
- **修改主配置文件**：`E:\soft\nginx-1.25.3\conf\nginx.conf`
  - 在`http`块末尾添加以下指令：
    ```nginx
    include conf.d/*.conf;  # 加载conf.d目录下所有子配置
    ```
  - *作用*：自动加载所有子配置文件，无需手动修改主配置（参考网页1、22）。

#### **4. 验证与重载配置**
1. **检查语法**：
    ```bash
    nginx -t -c E:\soft\nginx-1.25.3\conf\nginx.conf
    ```
    - 输出`successful`表示配置正确（参考网页19）。

2. **重载配置**：
    ```bash
    nginx -s reload
    ```
    - 平滑重载，不影响现有连接（参考网页19）。

#### **5. 优先级与冲突处理**
- **文件名顺序**：子配置文件按文件名顺序加载，建议用数字前缀（如`01-vue.conf`）控制加载顺序（参考网页22）。
- **端口冲突**：若8848端口被占用，需修改子文件中的`listen`值（参考网页1中的端口检查方法）。

---

### **最终文件路径与验证**
| 文件/目录                | 路径                                  | 作用                         |
|--------------------------|---------------------------------------|------------------------------|
| 子配置文件目录           | `E:\soft\nginx-1.25.3\conf\conf.d`   | 存放所有子配置文件           |
| Vue代理子文件            | `E:\soft\nginx-1.25.3\conf\conf.d\vue-proxy.conf` | 独立管理代理与静态资源配置   |
| 主配置文件               | `E:\soft\nginx-1.25.3\conf\nginx.conf`           | 通过`include`引入子配置      |

**验证步骤**：
1. 访问 `http://localhost:8848`，确认Vue页面正常加载。
2. 在前端发起`/api/user`请求，观察是否代理到`www.wsjtest.com/user`（通过浏览器开发者工具查看网络请求）。

---

### **引用说明**
- 子配置文件管理：
- 反向代理路径重写：
- 配置加载顺序：
- 语法检查与重载：
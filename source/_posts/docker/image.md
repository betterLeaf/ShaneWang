---
title: 镜像
date: 2024-12-07 11:12:33
tags:  
    - docker
    - docker入门
    - 镜像
cover: https://i.ibb.co/xDn34pF/docker-default-meta-image-1110x583.webp
categories: 容器&部署
---




## 镜像

### 镜像是什么

镜像是Docker容器运行的基础，它是一个只读的模板。镜像可以用来创建Docker容器。Docker提供了一个很简单的机制来创建镜像或者更新现有的镜像，用户也可以从网上下载已经创建好的镜像。

### 如何创建一个镜像

下面创建一个vue3项目镜像, 目录结构如下


``` 
my-vue-app/
├── Dockerfile
├── .dockerignore
├── package.json
├── package-lock.json
├── src/
└── public/

```

#### 添加Dockerfile
Dockerfile的作用 用于定义如何构建镜像的文件


下面在项目（如vue3）根目录下创建一个名为 Dockerfile 的文件，并添加以下内容：

```dockerfile
# 使用官方Node.js镜像作为基础镜像
FROM node:16 AS build

# 设置工作目录
WORKDIR /app

# 复制 package.json 和 package-lock.json
COPY package*.json ./

# 安装依赖
RUN npm install

# 复制项目文件（移动到工作目录/app）
COPY . .

# 构建项目
RUN npm run build

# 使用Nginx作为生产环境的服务器
FROM nginx:alpine

# 复制构建的文件到Nginx的html目录
COPY --from=build /app/dist /usr/share/nginx/html

# 暴露端口
EXPOSE 80

# 启动Nginx
# nginx 启动nginx服务器
# -g 设置全局指令
# daemon off; nginx以前台模式运行，而不是默认的后台模式
CMD ["nginx", "-g", "daemon off;"]
```
#### 添加.dockerignore

用于指定在构建镜像时要排除的文件和目录，以减少镜像体积和加快构建速度。

```bash
node_modules
dist
npm-debug.log
*.log
.git
```

#### 构建Docker镜像
在项目根目录下运行以下命令来构建镜像：

```bash
# -t 指定镜像名称和标签，这里我们使用 my-vue-app 作为镜像名称，latest 作为标签
# my-vue-app:latest 表示镜像名称和标签
# . 表示当前目录 (Dockerfile 所在目录)
docker build -t my-vue-app .
```

#### 运行Docker容器

```bash
# -d 指定容器以分离模式运行
# -p 8080:80 指定端口映射，将容器的 80 端口映射到主机的 8080 端口
# --name my-vue-app-container 指定容器名称为 my-vue-app-container
# my-vue-app 指定要运行的镜像名称，即刚才构建的镜像
docker run -d -p 8080:80 --name my-vue-app-container my-vue-app
```

### 镜像的分层


### 镜像的导入和导出


### 镜像的保存和加载


### 镜像的删除



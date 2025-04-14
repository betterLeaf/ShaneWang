---
title: 在 VSCode 中调试 NestJS 应用
date: 2024-11-02 15:07:49
tags:
    - node
    - nestjs
cover: https://i.ibb.co/jZVD0dgd/nest-big-logo.png
categories: 后端开发
---

本文将介绍如何在 VSCode 中配置和调试 NestJS 应用，通过以下简单步骤，您可以轻松实现断点调试功能。

### 1. 创建调试配置文件
首先，在 VSCode 中打开调试面板，点击创建 launch.json 文件：
![创建调试配置文件](https://i.ibb.co/bTvhMDX/i-Shot-2025-04-14-10-21-40.png)

### 2. 选择运行环境
在弹出的环境选择框中，选择 "Node.js" 作为调试环境：
![选择Node.js环境](https://i.ibb.co/bqN7rgb/i-Shot-2025-04-14-10-22-16.png)

### 3. 配置启动方式
在调试配置选项中，选择 "通过 npm 启动"，这样可以使用项目中定义的 npm 脚本：
![选择npm启动方式](https://i.ibb.co/mVdCb63r/i-Shot-2025-04-14-10-24-00.png)

### 4. 调整调试配置
确认并根据需要修改 launch.json 文件中的配置参数：
![调整调试配置](https://i.ibb.co/1fhqL3T7/i-Shot-2025-04-14-10-44-17.png)

### 5. 开始调试
点击调试按钮或使用快捷键 F5 启动调试模式：
![启动调试模式](https://i.ibb.co/8LLGmVmC/i-Shot-2025-04-14-10-46-49.png)

### 6. 验证运行状态
启动完成后，在浏览器中访问 `http://localhost:3000`，确认应用正常运行：
![验证应用运行状态](https://i.ibb.co/CpdYrHH8/i-Shot-2025-04-14-10-25-16.png)

至此，您已经成功配置了 NestJS 应用的调试环境。现在您可以在代码中设置断点，实时监控变量状态，更高效地进行开发和调试工作。




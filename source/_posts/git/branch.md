---
title: Branch & Tag
date: 2024-10-29 08:56:10
tags:
    - git

cover: https://i.ibb.co/YfBqspD/banner-2.png
categories: git
---



在 Git 中，分支（branch）和标签（tag）是两种重要的概念，它们各自有不同的作用和特点。

## 分支（Branch）

1. **概念**：
   - 分支是代码的一个独立开发线。主分支通常是 `main` 或 `master`，其他分支用于开发新功能、修复bug等。

2. **作用**：
   - **并行开发**：允许多个开发者同时在不同的功能上工作，而不会互相干扰。
   - **实验性开发**：可以在分支上进行实验，不会影响主分支的稳定性。
   - **合并功能**：完成开发后，可以将分支合并到主分支中，整合新功能。

3. **生命周期**：
   - 分支可以创建、删除和合并，通常是短期存在的。

## 标签（Tag）

1. **概念**：
   - 标签是对特定提交的标记，通常用于表示项目的某个重要节点，如发布版本。

2. **作用**：
   - **版本控制**：用于标记发布版本，如 `v1.0`、`v2.0` 等，方便后续查找和管理。
   - **不可变性**：标签一旦创建，通常不会被移动，代表了一个固定的状态。

3. **生命周期**：
   - 标签是静态的，通常在创建后不会再更改或删除。它们用于记录历史状态。

## 总结

- **分支**：用于并行开发和实验，具有动态生命周期。
- **标签**：用于标记特定版本，具有静态性质。

这两者结合使用，可以有效管理代码的版本和开发流程。

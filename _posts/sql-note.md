---
title: 数据库原理与应用课程笔记
categories:
  - 笔记
tags:
  - 数据库
  - 课程
date: 2020-04-15 10:00:00
---

# 数据库系统概论

## 数据管理技术发展

人工管理 -> 文件系统管理 -> 数据库系统管理

### 人工管理

特点：

- 数据面向应用
- 数据不保存
- 数据不能共享
- 不具有数据独立性

### 文件管理

特点：

- 面向应用
- 具有一定的独立性
- 可以长期保存

## 数据库的概念

**设计理念:** 它应该能够像操作系统屏蔽了硬件访问复杂性那样，屏蔽数据访问的复杂性。由此产生了数据管理系统，即数据库。

**长期储存在计算机内，有组织的、统一管理的、可共享的相关数据的集合**

## 体系结构

体系结构分三级

- 用户视图（外部级别）（外模式）（数据库表的访问
- 全局视图（概念级）（概念模式）（数据库的表定义
- 储存视图（内部级）（内模式）（数据库操作物理数据的策略

## 数据独立性

- 物理独立性 （应用程序与数据分离，相互独立，当物理储存改变时，程序无需改变
- 逻辑独立性 （应用程序与数据逻辑相互独立，数据的逻辑改变了，应用程序也可不变

数据库的三级模式，两级映射实现了**数据与程序之间的独立性**



---

**参考资料**

[数据库原理与应用 -- 孙孔峰、王舒、曾志平、单缅 @中国大学Mooc](https://www.icourse163.org/spoc/learn/GDY345-1451774169)

[数据库@Wikipedia](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93)
---
title: overView
tags: 数据库
renderNumberedHeading: true
grammar_cjkRuby: true
---

# DBMS 系统概述

[**数据库管理系统**](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%BA%93%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F)（Database Management System，DBMS）是一种操纵和管理数据库的大型软件，用于建立、使用和维护数据库，简称DBMS。

## 数据库管理系统概述
![概述图](https://raw.githubusercontent.com/mousezheng/source-code-analysis/master/img/2020_8_20_数据库系统实现_1597890447178.png)
	
### 查询处理概述

> 查询处理指：“可能影响数据库内容，或者获取数据，但不影响数据库模式”。

- 查询响应
	- 查询编译器（分析优化查询，得到查询计划）
	- 执行引擎（操作资源管理器）
	- 操作管理器（存储数据文件，文件数据格式和记录大小以及索引文件等）
	- 缓冲区管理器（从持久存储数据的辅助器中将部分数据存储到缓冲区中，比如从磁盘中将高频访问数据放入主存中）
	- 存储管理器（可以作为磁盘控制器的抽象，缓冲区管理器需要通过存储管理器访问磁盘）
- 事务处理（将一组若干查询或其他动作，作为一个执行单位原子并且孤立的执行）

---
title: INNODB关键特性
date: 2016-09-08 16:17:54
tags: mysql
categories: mysql
---
# INNODB关键特性

INNODB关键特性包括如下：
1. 插入缓冲（insert buff）
2. 两次写（double write）
3. 自适应哈希索引（Adaptive Hash Index）
4. 异步IO(Async IO)
5. 刷新邻接页（Flush Neighbor Page）

### 插入缓冲
插入缓冲带来了性能上的提升

### 两次写
带给innodb数据页的可靠性
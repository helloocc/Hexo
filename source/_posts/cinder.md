---
title: Cinder
date: 2018-05-20 13:50:17
tags:
---


## Cinder的内部架构


- 三个主要组成部分

	- cinder-api 组件负责向外提供Cinder REST API

	- cinder-scheduler 组件负责分配存储资源

	- cinder-volume 组件负责封装driver，不同的driver负责控制不同的后端存储



- 组件之间的RPC靠消息队列（Queue）实现

- Cinder的开发工作主要集中在scheduler和driver，以便提供更多的调度算法、更多的功能、以及指出更多的后端存储

- Volume元数据和状态保存在Database中

![cinder-framework][https://raw.githubusercontent.com/helloocc/Hexo/master/source/_posts/images/cinder-framework.png]

cinder内部各组件之间使用基于RabbitMQ的RPC通信。cinder-scheduler和cinder-volume分别会创建RPC连接，启动消费者线程，然后等待队列消息。当轮询查询到消息到达后，创建线程处理相关消息。








## 基本场景

![cinder-scenario][https://raw.githubusercontent.com/helloocc/Hexo/master/source/_posts/images/cinder-scenario.png]

---
title: gitblit 开启工单功能
comments: true
categories:
  - 工具
tags:
  - git
  - Gitblit
  - 工单
date: 2018-12-28 15:04:38
keywords: Gitblit,工单
description: 介绍一下Gitblit 工单功能的开启方法
---

# Gitblit 工单功能

## 最终效果

网上看人家的`Gitblit`界面是这样的{% asset_img 1545978402898.png 工单功能开启后的效果 %}

> 有新建工单的功能，而我安装的git1.8.0 版本却找不到这个按钮

## 解决方法

在网上搜索一番，原来工单的功能并不是默认打开的。 
### 打开步骤
1. 找到你的配置文件`D:\gitblit-1.8.0\data\defaults.properties`
2. 搜索`tickets.service`
3. 为其赋值`tickets.service = com.gitblit.tickets.BranchTicketService`
4. 重启Gitblit

## 关于 tickets.service

tickets.service有三个取值：

1. 工单将保存在文件系统中

```properties
ptickets.service = com.gitblit.tickets.FileTicketService
```

2. 工单将使用一个版本库来保存

```properties
tickets.service = com.gitblit.tickets.BranchTicketService
```

3. 工单将使用Redis来保存

```properties
tickets.service = com.gitblit.tickets.RedisTicketService
```

> 此时需要搭配另外一个属性，例子：
 ```properties
 #注意：redis://:foobared@localhost:6379/2 这个url你得自己搭建redis才会获得 
 tickets.redis.url = redis://:foobared@localhost:6379/2
 ```
> 关于 [redis](https://redis.io/) 更多的内容，请自行google。


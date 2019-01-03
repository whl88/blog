---
title: Navicat Premium 连接oracle
comments: true
categories:
  - 工具
tags:
  - Navicat Premium
  - oracle
date: 2019-01-03 14:20:25
keywords: ORA-12514,ORA-12737
description: Navicat Premium 连接oracle报错。
---

# Navicat Premium 简介

大家可能对 Navicat 很熟悉，用它连接mysql很方便。Navicat Premium 是 Navicat 系列中的一个，只是它支持连接更多种数据库:
{% asset_img 1546491519392.png Navicat Premium 连接oracle %}

# 报错内容

今天在连接oracle数据库时报错了，
{% asset_img 1546492000622.png ORA-12514 %}

服务名或SID选到SID上报另外一个错，
{% asset_img 1546492174110.png ORA-12737 %}
# 解决方法

## 操作

打开 `工具 》选项 》其他 》 OCI`，指定一下OCI ，像下图一样。 {% asset_img 
1546496004829.png %}

## 解释
一下这一段来源于[官网](http://wiki.navicat.com/wiki/index.php/Instant_client_required)的翻译。
> Basic 和 TNS 连接类型需要 Instant Client 包。要下载 Instant Client
> 包（Instant Client Package - Basic），请前往
> http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html
>
> 注意：Navicat 版本 9 或以上，Navicat 捆绑了 instant client。 连接类型
> Basic 在 Basic 模式中，Navicat 通过 Oracle Call Interface (OCI) 连接
> Oracle。
>
> OCI是一个应用程序界面，让程序开发人员使用第三代语言原有进程或函数呼叫来访问 Oracle
> 数据库及控制全部 SQL 语句运行的阶段。OCI 是一个标准数据库访问的库和动态链接库形式检索函数。
>
>  TNS 在 TNS 模式中，Navicat Oracle 使用在 tnsnames.ora 文件中的别名项目通过 Oracle Call Interface
> (OCI) 连接 Oracle 服务器。 
>
> **Windows** 
>
> 安装说明 
>
> 注意：Navicat 版本 10 或以下，Navicat 只支持 32-bit instant client。
>
> 为你的平台下载相应的 Instant Client 包。所有安装需要 Basic 或 Basic Lite package。
>
> **<u>注意：</u>** 
>
> **<u>1.Oracle 9i 或以上，你需要 Instant Client 11 或以下</u>** 
>
> **<u>2.Oracle 8 和 8i 服务器，你需要 Instant Client 10 或以下</u>** 
>
> {% asset_img InstantClientSite.jpg %}
>
> 解压缩包到一个单一目录，例如 "C:\instantclient_11_1"。 
>
> 在选项 -> OCI 或环境，选择在步骤 2 定义目录的 oci.dll（"C:\instantclient_11_1\oci.dll"）。 
>
> 重新启动 Navicat。
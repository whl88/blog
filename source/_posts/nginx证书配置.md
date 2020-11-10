---
title: Nginx Https 配置
comments: true
categories:
  - 工具
tags:
  - Nginx
  - Https
date: 2020-11-10 10:58:19
keywords: Nginx,Https
description: Nginx Https 配置
---

# 以下属性中以ssl开头的属性代表与证书配置有关，其他属性请根据自己的需要进行配置。

```ini
server {
    listen 443 ssl;   #SSL协议访问端口号为443。此处如未添加ssl，可能会造成Nginx无法启动。
    server_name localhost;  #将localhost修改为您证书绑定的域名，例如：www.example.com。
    root html;
    index index.html index.htm;
    ssl_certificate cert/domain name.pem;   #将domain name.pem替换成您证书的文件名。
    ssl_certificate_key cert/domain name.key;   #将domain name.key替换成您证书的密钥文件名。
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;  #使用此加密套件。
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;   #使用该协议进行配置。
    ssl_prefer_server_ciphers on;   
    location / {
        root html;   #站点目录。
        index index.html index.htm;   
	}
} 
```

# Http 自动 转Https

```ini
server {
    listen 80;
    server_name localhost;   #将localhost修改为您证书绑定的域名，例如：www.example.com。
    rewrite ^(.*)$ https://$host$1 permanent;   #将所有http请求通过rewrite重定向到https。
    location / {
        index index.html index.htm;
    }
}
```


---
title:  nginx教程
heading: 如何在centos7安装nginx教程
date: 2020-04-10T03:22:22.943Z
categories: ["code"]
tags: ["nginx教程"]
description: 如何安装nginx
---

linux 安装nginx并配置

```bash
yum install -y epel-release
yum install -y nginx
systemctl enable nginx
systemctl start nginx
systemctl status nginx
curl localhost
# 记得开放端口
firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```


```dsconfig
 server {
        listen       80;
        server_name  localhost;

        location / {
            root   html;
            index  index.html index.htm;
            #proxy_pass http://127.0.0.1:9292/;
        }

        location /test/ {
            proxy_pass http://127.0.0.1:8093/test/;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```
---
title: '阿里云域名 docker下申请泛域名'
date: Thu, 10 Jun 2021 15:54:00 +0000
draft: false
slug: 578 
tags: ['docker', 'NAS物语', '原创教程', '域名']
---

构建
==

```
Git pull https://github.com/monkeyWie/certbot-dns-aliyun.git
cd docker
docker build -t certbot-aliyun:latest .
```

启动容器
====

docker官方仓库有这个镜像

```
docker run \
--name cert \
-itd \
-v /etc/letsencrypt:/etc/letsencrypt \
-e ACCESS_KEY_ID=XXX \
-e ACCESS_KEY_SECRET=XXX \
liwei2633/certbot-aliyun
```

首次创建证书，根据命令提示输入，多个域名用,隔开
========================

```
docker exec -it cert ./create.sh *.pdown.org
```

续签
==

```
docker exec cert ./renew.sh
```

### 记录申请通配符的流程

![](https://gao4.top/wp-content/uploads/2021/06/2021-06-08_171602.png)

### 证书路径

```
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/gao4.top/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/gao4.top/privkey.pem
```
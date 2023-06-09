---
title: 'FreshRSS docker-compose.yml 教程'
date: Sun, 14 Mar 2021 17:30:38 +0000
draft: false
slug: 478 
tags: ['docker', 'docker-compose', 'freshrss', 'NAS物语', '原创教程']
---

创建docker-compose.yml文件

```
mkdir /home/FreshRSS && cd /home/FreshRSS
nano docker-compose.yml
```

填入以下内容 内容来源于[官方GIthub](https://github.com/FreshRSS/FreshRSS/blob/master/Docker/docker-compose.yml)

```
version: "3"

services:
  freshrss-db:
    image: postgres:12-alpine
    container_name: freshrss-db
    hostname: freshrss-db
    restart: unless-stopped
    volumes:
      - ./db:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: freshrss
      POSTGRES_PASSWORD: freshrss
      POSTGRES_DB: freshrss

  freshrss-app:
    image: freshrss/freshrss:latest
    container_name: freshrss-app
    hostname: freshrss-app
    restart: unless-stopped
    ports:
      - "8080:80"
    depends_on:
      - freshrss-db
    volumes:
      - ./data:/var/www/FreshRSS/data
      - ./extensions:/var/www/FreshRSS/extensions
    environment:
      CRON_MIN: '*/20'
      TZ: Asia/Shanghai

volumes:
  db:
  data:
  extensions:
```

打开浏览器访问http://IP:8080进入安装界面 支持中文

*   数据库选择postgres
*   数据库地址填freshrss-db
*   数据库用户名为freshrss
*   数据库密码为freshrss
*   这些都是上面文件环境变量按需修改
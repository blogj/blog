---
title: 'FileRun docker-compose 安装搭建'
date: Sun, 27 Sep 2020 12:17:45 +0000
draft: false
slug: 288 
tags: ['FileRun', '同步', '网盘', '资料', '转载学习']
---

> 先说一下结论，安装后感觉确实比nextcloud快，特别对于ody备份照片来说，但有一个需要注意，在2020年官方的安卓客户端已经暂停更新，现在用的是nextcloud的安卓客户端，但默认登陆进去会有一个@Home目录，没有直接进入/目录，对客户端有些不兼容。现还没找到解决办法，只能等官方更新

```
version: '2'

services:
  db:
    image: mariadb:10.1
    environment:
      MYSQL_ROOT_PASSWORD: your_mysql_root_password
      MYSQL_USER: your_filerun_username
      MYSQL_PASSWORD: your_filerun_password
      MYSQL_DATABASE: your_filerun_database
    volumes:
      - ./filerun/db:/var/lib/mysql

  web:
    image: afian/filerun
    environment:
      FR_DB_HOST: db
      FR_DB_PORT: 3306
      FR_DB_NAME: your_filerun_database
      FR_DB_USER: your_filerun_username
      FR_DB_PASS: your_filerun_password
      APACHE_RUN_USER: www-data
      APACHE_RUN_USER_ID: 33
      APACHE_RUN_GROUP: www-data
      APACHE_RUN_GROUP_ID: 33
    depends_on:
      - db
    links:
      - db:db
    ports:
      - "80:80"
    volumes:
      - ./filerun/html:/var/www/html
      - ./filerun/user-files:/user-files
```

参考链接
----

[官方文档很详细推荐参考](https://docs.filerun.com/docker)

[什么值得买社区文章](https://post.m.smzdm.com/p/ag82pn83/)

默认登陆用户
------

```
* Username: superuser

* Password: superuser
```

中文汉化设置
------

打开[汉化地址](https://raw.githubusercontent.com/filerun/translations/master/chinese.php) 文件 Ctrl ＋S 保存为php文件，在后台上传刷新，就是中文的了

![](https://gao4.top/wp-content/uploads/2020/09/20200927121441.png)
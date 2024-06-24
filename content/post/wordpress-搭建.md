---
title: 'wordpress 搭建'
date: Fri, 31 Jul 2020 20:10:08 +0000
draft: false
slug: 27 
tags: ['wordpress', '转载学习']
---

Github下载文件
----------

```
git clone https://github.com/kefins/docker_wpress.git wpress
```

修改文件

```
cd wpress
vi docker-compose.yml
```

修改1个地方image: wordpress:php7.4-fpm

```
version: '3'
services:

    mysql:
        image: mariadb
        container_name: mariadb
        ports:
            - '3306:3306'
        volumes:
            - ./sqldb:/var/lib/mysql
        environment:
            - MYSQL_ROOT_PASSWORD=UNJ^)-+566YYunfn
            - MYSQL_DATABASE=wordpress
            - MYSQL_USER=wordpress
            - MYSQL_PASSWORD=aqwe8662
        networks:
            - backend
        restart: always

    wordpress:
        depends_on:
            - mysql
        image: wordpress:php7.4-fpm
        container_name: wordpress
        ports:
            - '9000:9000'
        volumes:
            #- ./php-fpm:/usr/local/etc/php-fpm.d
            - ./www:/var/www/html
        environment:
            - WORDPRESS_DB_NAME=wordpress
            - WORDPRESS_TABLE_PREFIX=wp_
            - WORDPRESS_DB_HOST=mysql:3306
            - WORDPRESS_DB_USER=wordpress
            - WORDPRESS_DB_PASSWORD=aqwe558123
        links:
            - mysql
        networks:
            - frontend
            - backend
        restart: always

    nginx:
        image: nginx:latest
        container_name: nginx
        ports:
            - '80:80'
            - "443:443"
        volumes:
            - ./nginx:/etc/nginx/conf.d
            - ./logs/nginx:/var/log/nginx
            - ./www:/var/www/html
            - /var/run/docker.sock:/tmp/docker.sock:ro
        links:
            - wordpress
        networks:
            - frontend
        restart: always

networks:
    frontend:
        #name: frontend
        driver: bridge
    backend:
        #name: backend
        driver: bridge
```

启动容器

```
docker-compose  up -d
```

关闭自带防火墙
-------

```
关闭防火墙：
systemctl stop firewalld.service 
``````
禁用防火墙：
systemctl disable firewalld.sevice
```

打开云服务器安全组
---------

![](https://gao4.top/wp-content/uploads/2020/08/1537140-20190921212116311-2016901794-1024x374.png)

进入云平台

![](https://gao4.top/wp-content/uploads/2020/08/1537140-20190921212730589-895949023-1024x329.png)

添加安全组对应的服务器实例

![](https://gao4.top/wp-content/uploads/2020/08/1537140-20190921212454367-1479605288-1024x206.png)

自定义添加对应端口的入站规则

打开浏览器，输入x.x.x.x/wp-admin/install.php，就会看到WordPress的安装界面。至此，就完成了所有的部署工作。

![](https://api.yimian.xyz/img?type=moe)
---
title: 'NGINX 非443端口http跳转https'
date: Thu, 24 Jun 2021 10:32:00 +0000
draft: false
slug: 600 
tags: ['NAS', 'NAS物语', 'nginx', '原创教程', '端口']
---

关于Nginx这个web服务器软件在NAS上配置非443端口访问时候跳转https问题，默认80端口与443端口无法访问情况。

```
error_page 497 301 =307 https://$host:9443$request_uri;
```

添加上面代码就行https端口为9443这样就不会出现访问http出现错误的问题

### 完整代码

容器部署nginx桥接443端口为9443 然后反向代理192.168.100.4:86用wallabag.gao4.top:9443访问服务并让http跳转https

```
upstream dockername1 {
    server 192.168.100.4:86; # 端口改为docker容器提供的端口
}


server {
    listen 80;
    listen 443 default ssl;
    server_name  wallabag.gao4.top;
    error_page 497 301 =307 https://$host:9443$request_uri;
    gzip on;
    ssl_certificate /ssl/fullchain1.pem;
    ssl_certificate_key /ssl/privkey1.pem;

    # access_log /var/log/nginx/dockername_access.log combined;
    # error_log  /var/log/nginx/dockername_error.log;

    location / {
        proxy_redirect off;
        proxy_pass http://dockername1;

        proxy_set_header  Host                $http_host;
        proxy_set_header  X-Real-IP           $remote_addr;
        proxy_set_header  X-Forwarded-Ssl     on;
        proxy_set_header  X-Forwarded-For     $proxy_add_x_forwarded_for;
        proxy_set_header  X-Forwarded-Proto   $scheme;
        proxy_set_header  X-Frame-Options     SAMEORIGIN;

        client_max_body_size        100m;
        client_body_buffer_size     128k;

        proxy_buffer_size           4k;
        proxy_buffers               4 32k;
        proxy_busy_buffers_size     64k;
        proxy_temp_file_write_size  64k;
    }
}
```
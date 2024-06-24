---
title: 'tailscale docker上安装搭建方法'
date: Thu, 20 May 2021 13:31:00 +0000
draft: false
slug: 555 
tags: ['NAS物语', 'tailscale', 'VPN', '原创教程']
---

![](https://gao4.top/wp-content/uploads/2021/05/image-16.png)

启动

```
docker run -d --restart=always --name=tailscaled -v /var/lib:/var/lib -v /dev/net/tun:/dev/net/tun --network=host --privileged fastandfearless/tailscale
```

登录

```
docker exec tailscaled tailscale up
```

查看状态

```
docker exec tailscaled tailscale status
```

停止tailscale容器

```
docker stop tailscaled 
```

删除tailscale容器

```
docker rm tailscaled 
```

[参考官方docker编译文件](https://github.com/tailscale/tailscale/blob/main/Dockerfile)

[镜像地址](https://hub.docker.com/r/fastandfearless/tailscale/)
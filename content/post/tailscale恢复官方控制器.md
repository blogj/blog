---
title: "Tailscale恢复官方控制器"
date: 2023-11-05T18:24:03+08:00
draft: false
slug: 1001
tags: ['NAS物语', 'Tailscale', ' 内网穿透', '资料']
---

## 序言
在节点登录了第三方控制器后，想恢复官方控制器
## 解决办法
命令行输入以下命令
```
tailscale up --accept-dns=false --accept-routes --login-server=https://controlplane.tailscale.com --advertise-routes=192.168.100.0/24
```
重要的是```--login-server=https://controlplane.tailscale.com```

## Window删除第三方tailscale用户

登录后节点有第三方用户的缓存可以尝试删除
```
C:\ProgramData\Tailscale
C:\Users%USERNAME%\AppData\Local\Tailscale
C:\Windows\System32\config\systemprofile\AppData\Local\Tailscale
```
再重新安装登录
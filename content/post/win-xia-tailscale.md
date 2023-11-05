---
title: "unattended Tailscale"
date: 2023-11-05T18:10:57+08:00
draft: false
slug: 1000 
tags: ['NAS物语', 'Tailscale', 'Windows', ' 内网穿透', '资料']
---

## 序言

解决 Windows 下 Tailscale 未登录桌面不启动问题
新版本的 ui 启动无人值守的选项```run unattended mode```已经去掉，所以只能命令行设置

## 研究资料

https://tailscale.com/kb/1080/cli/#login 官方文档来源

## 解决办法

以管理员身份运行`cmd` 输入以下命令重新登录 tailscale

```
tailscale login --unattended
```
## 测试 
重新关机Window电脑，然后再开机不进入桌面 用手机登录管理后台发现Window节点已经在线


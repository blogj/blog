---
title: 'ESXI 群晖下连接VPN网络报错解决'
date: Thu, 29 Apr 2021 16:25:03 +0000
draft: false
slug: 494 
tags: ['NAS', 'NAS物语', 'VPN', '原创教程', '群晖']
---

前言

我的vpn服务端搭建在openwrt软路由上使用SoftEther作为相关软件，在群晖下出现

群晖 l2tp连接失败 请检查。。。。解决办法如下勾选第二个选项就行

![](https://gao4.top/wp-content/uploads/2021/04/SAVE_20210429_162054.jpg)

啰嗦一下环境

*   esxi6.5u2
*   DS3617XS
*   MAC已修改
*   tun未开启
*   宿主机戴尔r410
*   网卡为博通网卡

为啥我判断是群晖的问题嘞，win10最新，正常连接 安卓10正常连接，白群晖正常连接
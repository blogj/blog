---
title: 'NPS 内网穿透工具学习记录'
date: Sun, 27 Dec 2020 22:48:12 +0000
draft: false
slug: 368 
tags: ['NPC', 'NPS', '内网穿透', '原创教程']
---

介绍
--

2021年5月18日更新

官方更新了docker安装方式，推荐用这个方法强烈推荐

[地址](https://hub.docker.com/r/ffdfgdfg/nps)

nps是一款轻量级、高性能、功能强大的**内网穿透**代理服务器。目前支持**tcp、udp流量转发**，可支持任何**tcp、udp**上层协议（访问内网网站、本地支付接口调试、ssh访问、远程桌面，内网dns解析等等……），此外还**支持内网http代理、内网socks5代理**、**p2p等**，并带有功能强大的web管理端。支持中文

### 官方文档

[https://ehang-io.github.io/nps/#/](https://ehang-io.github.io/nps/#/)

![](https://gao4.top/wp-content/uploads/2020/12/Snipaste_2020-12-27_13-39-28-1024x620.png)

登录界面

安装
--

[https://github.com/ehang-io/nps/releases](https://github.com/ehang-io/nps/releases)

去Github下载对应计算机架构系统版本的服务器包

![](https://gao4.top/wp-content/uploads/2020/12/Snipaste_2020-12-27_13-46-16.png)

部分架构包图

### Github加速下载

[https://d.serctl.com/](https://d.serctl.com/)

安装为系统服务
-------

```
./nps install
```

![](https://gao4.top/wp-content/uploads/2020/12/Snipaste_2020-12-27_13-53-47.png)

### 注意事项

看下面这句话

![](https://gao4.top/wp-content/uploads/2020/12/Snipaste_2020-12-27_13-56-24.png)

```
2020/12/27 13:52:08当前目录中的静态文件和配置文件将无用
2020/12/27 13:52:08新的配置文件位于/etc/nps中，您可以对其进行编辑
注意客户端与服务端版本一样
```

使用systemctl来控制启动服务端
-------------------

```
sudo vi /lib/systemd/system/nps.service
``````
[Unit]
Description=nps service
After=network.target syslog.target
Wants=network.target

[Service]
Type=simple
ExecStart=/usr/bin/nps start

[Install]
WantedBy=multi-user.target
```

*   启动nps：`sudo systemctl start nps`
*   打开自启动：`sudo systemctl enable nps`
*   重启应用：`sudo systemctl restart nps`
*   停止应用：`sudo systemctl stop nps`
*   查看应用：`sudo systemctl status nps`

### 注意事项

现在NPS的配置文件在/etc/nps中，不用在当前目录配置 可以删除了

### 默认端口

*   nps默认配置文件使用了80，443，8080，8024端口
*   80与443端口为域名解析模式默认端口
*   8080为web管理访问端口
*   8024为网桥端口，用于客户端与服务器通信

访问web控制端
--------

http://公网ip:8080

注意 需要开启防火墙端口，云服务器也需要安全组放行

客户端安装与systemctl来控制启动
--------------------

### 注意事项

[注意看官方文档](https://ehang-io.github.io/nps/#/use?id=%E6%B3%A8%E5%86%8C%E5%88%B0%E7%B3%BB%E7%BB%9F%E6%9C%8D%E5%8A%A1)，很完善

下面报错是因为systemctl找不到npc二进制启动文件，指定就行

![](https://gao4.top/wp-content/uploads/2020/12/Snipaste_2020-12-27_22-38-23.png)

启动报错

安装客户端
-----

解压后执行

```
./npc install
```

复制配置文件

```
cp conf/npc.conf /etc/npc/
```

### 撰写service文件

```
sudo vim  /usr/lib/systemd/system/npc.service
```

### 文件内容

```
[Unit]
Description=npc service
After=network.target syslog.target
Wants=network.target

[Service]
Type=simple
ExecStart=/usr/bin/npc -config=/etc/npc/npc.conf
ExecStop=/usr/bin/npc stop

[Install]
WantedBy=multi-user.target 
```

\## 启动与设置开机自启动
--------------

```
systemctl start npc.service
systemctl enable npc.service
```
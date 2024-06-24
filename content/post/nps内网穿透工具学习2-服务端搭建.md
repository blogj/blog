---
title: 'NPS内网穿透工具学习2-服务端搭建'
date: Mon, 28 Dec 2020 10:44:52 +0000
draft: false
slug: 388 
tags: ['NPS', '内网穿透', '原创教程', '资料']
---

### 序言

通俗约定您已经，通晓基本的Linux知识（解压复制粘贴移动）、了解基本的网络知识（知晓什么是公网IP什么是内网IP）登录已root用户为准

#### 版本

*   [v0.26.9](https://github.com/ehang-io/nps/releases/tag/v0.26.9)
*   [linux\_amd64\_server.tar.gz](https://github.com/ehang-io/nps/releases/download/v0.26.9/linux_amd64_server.tar.gz)
*   centos7
*   已关闭Firewalld

### 下载解压安装

```
mkdir /home/nps
cd /home/nps
wegt https://github.com/ehang-io/nps/releases/download/v0.26.9/linux_amd64_server.tar.gz
tar -zxvf linux_amd64_server.tar.gz
./nps install
```

### 撰写程序NPS配置文件

打开配置文件

vim /etc/nps/nps.conf

### 撰写**systemd**配置文件

vim创建service文件

```
vim /usr/lib/systemd/system/nps.service
```

填入以下内容

```
[Unit]
Description=nps service
After=network.target syslog.target
Wants=network.target

[Service]
Type=simple
ExecStart=/usr/bin/nps start
ExecStop=/usr/bin/nps stop

[Install]
WantedBy=multi-user.target
```

### 启动并设置开机自启动

```
sudo systemctl start nps.service
sudo systemctl enable nps.service
```

*   启动nps：`sudo systemctl start nps`
*   打开自启动：`sudo systemctl enable nps`
*   重启应用：`sudo systemctl restart nps`
*   停止应用：`sudo systemctl stop nps`
*   查看应用：`sudo systemctl status nps`

### 访问设置

关闭防火墙并设置关闭开机启动

```
systemctl stop firewalld.service
systemctl disable firewalld.service 
```

开启云服务器安全组访问端口nps默认配置文件使用了80，443，8080，8024端口添加 80、443、8080、8024，这些端口都可以在`/etc/nps/nps.comf`修改

![](https://gao4.top/wp-content/uploads/2020/12/2020070811380775.png)

访问web控制面板

```
http://公网ip:8080
```

用户名密码

```
默认用户名:admin
默认密码:123
```

> 注意事项，如果访问不了用命令`sudo systemctl status nps`查看一下程序是否启动，如果没有启动成功是否443、80端口冲突在nps配置文件修改成其他端口就行，

### 后续

[官方文档](https://ehang-io.github.io/nps/#/)有很详细的描述更多请访问
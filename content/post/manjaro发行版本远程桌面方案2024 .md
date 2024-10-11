---
title: "Manjaro发行版本远程桌面方案2024 "
date: 2024-10-11T14:30:50+08:00
draft: false
slug: 20241011
tags: ['sunshine', '远程桌面', 'Manjaro', 'Linux']
---

## 预览图
![预览图](https://gao4.top/wp-content/uploads/2024/manjaro/Snipaste_2024-10-11_14-31-42.png)

## 前言
一直在全网寻找一个不卡的远程桌面方案所以就找到到了这一个远程游戏的解决方法非常合适win去远程Linux发行版本


## 优点
非常流程    非常流程   非常流程
130公里 双边移动宽带ipv6 通过wg远程 1080 60fps  也就是说能做到60帧率基本上和副屏幕差不多了
![预览图](https://gao4.top/wp-content/uploads/2024/manjaro/Snipaste_2024-10-11_14-36-50.png)

## manjaro下安装方法
**注意都是在root用户下执行下面命令**  
直接去github去下载安装包是是启动不成功的，搜索引擎说可以安装sunshine-git版本，可惜我不会 提供一个网上的包 不放心可以去试试sunshine-git版本


https://0x0.st/XE4O.tar.zst 这是包（如果你不相信我，就不要使用它）

重命名为 sunshine-0.23.1-5-x86_64.pkg.tar.zst **安装命令如下**

```
sudo pacman -U sunshine-0.23.1-5-x86_64.pkg.tar.zst

```
启动命令为
```
sunshine
```

默认访问为本机访问所以需要ssh隧道 默认端口为https://localhost:47990/ 因为我们启动了ssh隧道就访问https://localhost:47991/ 隧道设置如下图
![隧道](https://gao4.top/wp-content/uploads/2024/manjaro/Snipaste_2024-10-11_14-45-47.png)


### Linux下自启动设置
[官方文档](https://docs.lizardbyte.dev/projects/sunshine/en/latest/about/setup.html#install)
编辑
```markdown
nano /etc/systemd/system/sunshine.service
```
内容如下
```markdown
[Unit]
Description=Sunshine self-hosted game stream host for Moonlight.
StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
ExecStart=/usr/bin/sunshine
Restart=on-failure
RestartSec=5s
#Flatpak Only
#ExecStop=flatpak kill dev.lizardbyte.sunshine

[Install]
WantedBy=multi-user.target
```
在关闭软件情况下依次执行下面相关命令
```markdown
systemctl daemon-reload
systemctl enable sunshine.service
systemctl start sunshine.service
```
### 注意事项
注意客户端连接需要**moonlight**这个软件 下载连接如下  
https://flowus.cn/lecheng/share/cf8b9092-8ac2-4dd9-bb81-701be6e2f196
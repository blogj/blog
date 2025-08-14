---
title: 'Openwrt OpenConnect VPN 搭建设置教程'
date: Wed, 30 Sep 2020 13:52:00 +0000
slug: 293
tags: ['NAS物语', 'Openwrt', 'VPN', '原创教程']
---

\## 2022年1月4日更新如果luci没有界面可以去软件源手动下载安装 流程如下图

 ![流程图](https://gao4.top/wp-content/uploads/2022/01/851998809.png)

怎么知道当前openwrt架构的软件源包去后台看看就好了
 ![复制浏览器搜索](https://gao4.top/wp-content/uploads/2022/01/816834899.png)

序言
--

本来这个需求基本上可以说是可以用其他vpn来解决的，可我ody勒就是不走寻常路，局域网随便一台有docker的Linux服务器开一个vpn服务不香吗，非需要在openwrt上折腾，而这方面的资料很少，而且国内对openwrt的软件服务器支持很糟糕。没有开全局基本上都是GG。固件来源是Github云编译的固件

#### 需求

*   openwrt软路由承担服务
*   安全访问局域网
*   Udp加速访问
*   全平台客户端

#### 软件了解

OpenConnect是一个开源的应用程序最初是拿来做思科专有的Vpn客户端[AnyConnect](https://en.wikipedia.org/wiki/AnyConnect) 的开源替代品

OpenConnect项目还提供与 AnyConnect 兼容的服务器**，ocserv**

安装Ocserv服务端
-----------

如果web界面搜索不到安装用命令行安装

```
opkg update
opkg install ocserv luci-app-ocserv
reboot
```

第一步
---

**OpenWRT - 网络 - 防火墙 - 自定义规则 添加防火墙规则**

```
iptables -t nat -I POSTROUTING -s 192.168.100.0/24 -j MASQUERADE
iptables -I FORWARD -i vpns+ -s 192.168.100.0/24 -j ACCEPT
iptables -I INPUT -i vpns+ -s 192.168.100.0/24 -j ACCEPT
```

![自定义规则](https://gao4.top/wp-content/uploads/2020/09/image.png)

第二步
---

**OpenWRT - 网络 - 防火墙 -**端口转发 转发至外网

**转发端口4443** 默认端口为4443当然可以更改，

![](https://gao4.top/wp-content/uploads/2020/09/image-1-1024x405.png)

转发默认4443端口

第三步
---

开启服务点击**Enable server**这个

![](https://gao4.top/wp-content/uploads/2020/09/image-2-1024x367.png)

第四步
---

添加用户注意事项，需要开启服务后才能添加用户

![](https://gao4.top/wp-content/uploads/2020/09/image-3-1024x248.png)

注意事项，需要开启服务后才能添加用户没有开起服务的界面

添加一个实验用户ac密码为123456

![](https://gao4.top/wp-content/uploads/2020/09/image-4-1024x274.png)

客户端访问
-----

2021年1月28日添加**把勾去掉**

![](https://gao4.top/wp-content/uploads/2021/01/Snipaste_2021-01-28_20-05-44.png)

win10客户端访问**ip地址加端口点击connect**

![](https://gao4.top/wp-content/uploads/2020/09/image-5.png)

点击

![](https://gao4.top/wp-content/uploads/2020/09/image-6.png)

输入用户名**ac点击OK**

![](https://gao4.top/wp-content/uploads/2020/09/image-7.png)

输入密码点击Ok

![](https://gao4.top/wp-content/uploads/2020/09/image-8.png)

弹出这个点击Accept

![](https://gao4.top/wp-content/uploads/2020/09/image-9.png)

继续点击

![](https://gao4.top/wp-content/uploads/2020/09/image-10.png)

成功连接

![](https://gao4.top/wp-content/uploads/2020/09/Snipaste_2020-09-29_20-27-16.png)

客户端下载
-----

**第三方下载链接**（推荐）

https://ocserv.yydy.link:2023/#/

**Windows 10 操作系统**  
AnyConnect Mobile 4.8.02045

[点击下载](https://Gigss.coding.net/s/cbd9e911-b39c-4f76-851e-be02f8b06111)

**Windows 7/8 操作系统**  
AnyConnect Mobile 4.7.04056

[点击下载](https://Gigss.coding.net/s/7fe78da2-9340-48be-9f62-a665dcb5f5d4)

**Android 客户端**  
AnyConnect

[点击下载](https://Gigss.coding.net/s/85e68257-d3fc-4f3c-8612-826e2fa36313)

连接Openwet互相访问
-------------

添加路由

![](https://gao4.top/wp-content/uploads/2021/01/Snipaste_2021-01-28_19-43-00.png)

**Routing table**

防火墙规则

![](https://gao4.top/wp-content/uploads/2021/01/Snipaste_2021-01-28_19-43-32-1024x534.png)

测试

客户端测试

![](https://gao4.top/wp-content/uploads/2021/01/IMG_20210128_194456-988x1024.jpg)

客户端  

![](https://gao4.top/wp-content/uploads/2021/01/IMG_20210128_195241-1016x1024.jpg)

结果

内网局域网测试

![](https://gao4.top/wp-content/uploads/2021/01/Snipaste_2021-01-28_19-53-27.png)
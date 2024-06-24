---
title: 'UNraid 安装后注意事项常见操作'
date: Mon, 31 Aug 2020 13:18:41 +0000
draft: false
slug: 234 
tags: ['NAS物语', 'Unraid', '原创教程']
---

引导模式
----

UnRaid如果不能成功引导试试换传统引导模式

修改访问iP地址
--------

```
如果引导成功后访问不了，可能是局域网没有dhcp服务器，试试在Unraid引导选择界面选择启动桌面修改IP地址
```

![](https://gao4.top/wp-content/uploads/2020/08/20200831094440-1024x578.png)

修改ip界面

修改主机时间
------

修改主机时间为Beijing

![](https://gao4.top/wp-content/uploads/2020/08/20200831130330.png)

步骤1

![](https://gao4.top/wp-content/uploads/2020/08/20200831130529-1024x561.png)

步骤2

设置硬盘阵列开机自启动
-----------

默认情况下硬盘阵列是不会开机就启动的需要设置一下

![](https://gao4.top/wp-content/uploads/2020/08/20200831130758.png)

步骤1

![](https://gao4.top/wp-content/uploads/2020/08/20200831130825.png)

步骤二，切换为YES

安装应用商店插件
--------

![](https://gao4.top/wp-content/uploads/2020/08/20200831131237.png)

在后台进入plugins设置页，然后进入install plugin的tab下，输入对应的插件安装地址，点击“安装”按钮，静静等待它安装完就行了

**这里给出我安装的两个插件地址**

> **Docker市场**  
> [https://raw.githubusercontent.com/Squidly271/community.applications/master/plugins/community.applications.plg](https://links.jianshu.com/go?to=https%3A%2F%2Fraw.githubusercontent.com%2FSquidly271%2Fcommunity.applications%2Fmaster%2Fplugins%2Fcommunity.applications.plg)

> **查看未加入序列的硬件**  
> [https://github.com/dlandon/unassigned.devices/raw/master/unassigned.devices.plg](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fdlandon%2Funassigned.devices%2Fraw%2Fmaster%2Funassigned.devices.plg)

##### 无法安装插件的解决方案

安装软件商城出现问题，需要修改host指向国内的地址，具体host如下

方法1：通过vi 添加 hosts 中的地址“199.232.4.133 raw.githubusercontent.com”  
方法2：在终端中运行以下内容即可修改host

> echo "199.232.4.133 raw.githubusercontent.com" >> /etc/hosts

一键制作工具
------

[UnraidTool](https://gao4.top/wp-content/uploads/2020/08/6555e4d79948894.zip)[下载](https://gao4.top/wp-content/uploads/2020/08/6555e4d79948894.zip)

一键制作工具有问题，引导出错，建议下载包后用官方的制作工具制作启动盘然后用下面工具激活

![](https://gao4.top/wp-content/uploads/2021/01/Snipaste_2021-01-23_19-24-44.png)

key

**安装CA CONFIG EDITOR**
----------------------

一个在线配置文件编辑器

![](https://gao4.top/wp-content/uploads/2021/01/Snipaste_2021-01-23_20-19-03-1024x324.png)

编辑go文件

```
#!/bin/bash
# Start the Management Utility
/usr/local/sbin/emhttp &
modprobe i915
chown nobody:users /dev/dri
chmod 0777 /dev/dri/*
```
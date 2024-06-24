---
title: '群晖开启tun模块驱动以支持tinc'
date: Sun, 09 Aug 2020 09:27:29 +0000
draft: false
slug: 164 
tags: ['NAS', 'NAS物语', '群晖', '转载学习']
---

登陆群晖开启ssh，在终端依次用admin用户登陆再sudo -i 进入#管理员模式

检查tun模块状态
---------

```
lsmod | grep tun
```

如果结果为空，请尝试安装它
-------------

```
insmod /lib/modules/tun.ko
```

依次输入下列命令，确保tun.ko模块可以正常工作
-------------------------

```
mkdir /dev/net
mknod /dev/net/tun c 10 200
chmod 600 /dev/net/tun
cat /dev/net/tun
```

![](https://gao4.top/wp-content/uploads/2020/08/IMG_20200808_202841-1024x466.jpg)

命令结果

持久化操作
-----

模块开机自启动（不保证有效可以在群晖面板计划任务设置）

```
cat < /usr/local/etc/rc.d/tun.sh
#!/bin/sh -e
insmod /lib/modules/tun.ko
EOF
```

赋予脚本启动权限
--------

```
chmod a+x /usr/local/etc/rc.d/tun.sh
```

> tun/tap 驱动程序实现了虚拟网卡的功能，tun表示虚拟的是点对点设备，tap表示虚拟的是以太网设备，这两种设备针对网络包实施不同的封装。利用tun/tap 驱动，可以将tcp/ip协议栈处理好的网络分包传给任何一个使用tun/tap驱动的进程，由进程重新处理后再发到物理链路中。
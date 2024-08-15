---
title: 'Tailscale安装到ARM openwrt上'
date: Sun, 12 Jun 2022 12:09:10 +0000
draft: false
slug: 1076
tags: ['NAS物语', '资料', 'openwrt', 'Tailscale']
---

![](http://gao4.top/wp-content/uploads/2022/06/FotoJet-1024x576.png)

下载到tmp目录

```
wget https://pkgs.tailscale.com/stable/tailscale_1.70.0_arm64.tgz -P /tmp
```

解压压缩包并移动二进制文件

```
cd /tmp
tar x -zvf tailscale_1.70.0_arm64.tgz
cd tailscal_1.70.0_arm64
mv tailscale tailscaled /usr/sbin/
```

安装依赖文件

```
opkg update
opkg install ca-bundle kmod-tun
```

创建守护脚本文件

```
vim /etc/init.d/tailscale
```

写入如下代码

```
#!/bin/sh /etc/rc.common

# Copyright 2020 Google LLC.
# SPDX-License-Identifier: Apache-2.0

USE_PROCD=1
START=80

start_service() {
  /usr/sbin/tailscaled --cleanup

  procd_open_instance 
  procd_set_param command /usr/sbin/tailscaled

  # Set the port to listen on for incoming VPN packets.
  # Remote nodes will automatically be informed about the new port number,
  # but you might want to configure this in order to set external firewall
  # settings.
  procd_append_param command --port 41641

  # OpenWRT /var is a symlink to /tmp, so write persistent state elsewhere.
  procd_append_param command --state /etc/tailscale/tailscaled.state

  procd_set_param respawn
  procd_set_param stdout 1
  procd_set_param stderr 1

  procd_close_instance
}

stop_service() {
  /usr/sbin/tailscaled --cleanup
}
```

收尾工作

```
chmod +x /etc/init.d/tailscale
/etc/init.d/tailscale start
/etc/init.d/tailscale enable
```

tailscale相关命令

```
# 获取登录链接
tailscale up
#开启子网网路由
tailscale up --accept-routes=true  --accept-dns=false --advertise-routes=192.168.100.0/24
#查看状态
tailscale status
#查看ip
tailscale ip 
```
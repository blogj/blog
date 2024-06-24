---
title: 'Tinc全球虚拟局域网搭建记录'
date: Thu, 06 Aug 2020 19:10:04 +0000
draft: false
slug: 112 
tags: ['tinc', '内网穿透', '转载学习']
---

安装
--

```
apt-get install tinc
```

开启IPv4转发
--------

```
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf && sysctl -p
```

创建一个名为imlala的虚拟网络
-----------------

```
mkdir -p /etc/tinc/imlala && mkdir -p /etc/tinc/imlala/hosts
```

新建tinc.conf配置文件
---------------

```
vim.tiny  /etc/tinc/imlala/tinc.conf
```

配置文件内容
------

```
Name=imlala
Interface=vpn
Cipher=aes-256-cbc
Digest=sha512
ConnectTo=imlala
```

网络名就是imlala，网卡接口名vpn，以及加密方式

建hosts文件
--------

```
vim.tiny /etc/tinc/imlala/hosts/imlala
```

需要注意这个文件名必须和tinc.conf内的Name

写入如下配置
------

```
Address = 你的VPS公网IP
Subnet = 10.0.0.1/32
```

完成之后生成密匙对
---------

```
tincd -n imlala -K4096
```

按两下回车全部保持默认配置，生成完成之后，对应的文件路径

```
/etc/tinc/imlala/rsa_key.priv # 私钥
/etc/tinc/imlala/hosts/imlala # 公钥
```

配置虚拟网卡tinc-up
-------------

```
echo 'ip addr add 10.0.0.1/24 dev $INTERFACE' > /etc/tinc/imlala/tinc-up
``````
echo 'ip link set $INTERFACE up' >> /etc/tinc/imlala/tinc-up
```

给执行权限
-----

```
chmod +x /etc/tinc/imlala/tinc-*
```

启动
--

```
systemctl start tinc@imlala
systemctl enable tinc@imlala
```

**配置第二台**
---------

安装
--

```
apt-get install tinc
```

开启IPv4转发
--------

```
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf && sysctl -p
```

创建一个名为imlala2的虚拟网络
------------------

```
mkdir -p /etc/tinc/imlala2 && mkdir -p /etc/tinc/imlala2/hosts
```

新建tinc.conf配置文件
---------------

```
vim.tiny  /etc/tinc/imlala2/tinc.conf
```

配置文件内容
------

```
Name=imlala2
Interface=vpn
Cipher=aes-256-cbc
Digest=sha512
ConnectTo=imlala
```

网络名就是imlala2，网卡接口名vpn，以及加密方式

建hosts文件
--------

```
vim.tiny /etc/tinc/imlala2/hosts/imlala2
```

需要注意这个文件名必须和tinc.conf内的Name

写入如下配置
------

```
Subnet = 10.0.0.2/32
```

完成之后生成密匙对
---------

```
tincd -n imlala -K4096
```

按两下回车全部保持默认配置，生成完成之后，对应的文件路径

```
/etc/tinc/imlala2/rsa_key.priv # 私钥
/etc/tinc/imlala2/hosts/imlala # 公钥
```

配置虚拟网卡tinc-up
-------------

```
echo 'ip addr add 10.0.0.2/24 dev $INTERFACE' > /etc/tinc/imlala2/tinc-up
``````
echo 'ip link set $INTERFACE up' >> /etc/tinc/imlala2/tinc-up
```

给执行权限
-----

```
chmod +x /etc/tinc/imlala2/tinc-*
```

启动
--

```
systemctl start tinc@imlala2
systemctl enable tinc@imlala2
```
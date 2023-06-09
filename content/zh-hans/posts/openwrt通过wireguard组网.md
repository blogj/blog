---
title: 'openwrt通过wireguard组网'
date: Wed, 24 Feb 2021 15:01:46 +0000
draft: false
slug: 444 
tags: ['NAS物语', 'Openwrt', 'VPN', 'WG', 'Wireguard', '原创教程']
---

概念
--

网上找的图很好理解应该

![](https://gao4.top/wp-content/uploads/2021/02/Screenshot_2021-02-19-18-10-47-849_com.google.android.youtube-1024x473.jpg)

wireguard原理就不多做解释了，我们只是人为的分客户端与服务端，因为国情在这，有时候两端不可都是可以互相访问的公网。这是理论上最好的访问模型，但实际上使用也就服务端访问这种模式。

2021年5月21日更新

![](https://gao4.top/wp-content/uploads/2021/05/image-18.png)

### 公网环境

wireguard实际是支持ipv6的而我的环境是ipv4动态公网就不多做关于ipv6的介绍了，在2018年CN已经全面覆盖改造ipv6了，至少移动手机端与家庭宽带都支持ipv6了。

设备环境
----

*   OpenWrt R21.2.1
*   公网IPv4
*   移动手机端

openwrt端配置
----------

软件源 系统-软件包-发行版软件源

```
src/gz openwrt_core https://mirrors.cloud.tencent.com/lede/snapshots/targets/x86/64/packages
src/gz openwrt_base https://mirrors.cloud.tencent.com/lede/snapshots/packages/x86_64/base
src/gz openwrt_freifunk https://mirrors.cloud.tencent.com/lede/snapshots/packages/x86_64/freifunk
src/gz openwrt_helloworld https://mirrors.cloud.tencent.com/lede/snapshots/packages/x86_64/helloworld
src/gz openwrt_lienol https://mirrors.cloud.tencent.com/lede/snapshots/packages/x86_64/lienol
src/gz openwrt_luci https://mirrors.cloud.tencent.com/lede/releases/18.06.8/packages/x86_64/luci
src/gz openwrt_packages https://mirrors.cloud.tencent.com/lede/snapshots/packages/x86_64/packages
src/gz openwrt_routing https://mirrors.cloud.tencent.com/lede/snapshots/packages/x86_64/routing
src/gz openwrt_telephony https://mirrors.cloud.tencent.com/lede/snapshots/packages/x86_64/telephony
```

安装WG

```
opkg update
opkg install wireguard luci-app-wireguard luci-i18n-wireguard-zh-cn wireguard-tools
```

![](https://gao4.top/wp-content/uploads/2021/02/Snipaste_2021-02-24_15-01-01.png)

生成密钥
----

可以在终端下生成 也可以在各客户端下生成 privatekey\=私钥 publickey\=公钥

```
mkdir /etc/wireguard
cd /etc/wireguard
wg genkey | tee privatekey | wg pubkey > publickey
```

配置接口 依次找到[网络->接口](http://openwrt.lan/cgi-bin/luci/admin/network/network)\->添加新接口，设置内容如下

*   新接口名称：WG1
*   新接口协议：Wireguard VPN

![](https://gao4.top/wp-content/uploads/2021/02/Snipaste_2021-02-19_17-21-38-1024x386.png)

创建接口

接口基本设置

*   私钥 程序生成
*   监听接口 100
*   IP地址 就是接口的地址

**Peers**设置

*   公钥 就是 客户端的公钥 特别注意不是OPenwrt的对应的公钥
*   允许的 IP 设置VPN的路由
*   路由允许的 IP 勾选

防火墙设置

*   选择LAN

![](https://gao4.top/wp-content/uploads/2021/02/Snipaste_2021-02-19_17-33-33.png)

防火墙设置

防火墙 - 区域设置

![](https://gao4.top/wp-content/uploads/2021/02/Snipaste_2021-02-19_17-35-18-1024x635.png)

防火墙区域设置

建立通信规则添加端口

网络->防火墙->通信规则->添加，打开的页面为防火墙 - 通信规则 - 未命名规则，在基本设置中只需修改如下几项，其他项默认即可

*   名称：自定义
*   协议：UDP
*   源区域：任意区域
*   目标区域： 设备
*   目标端口： 100 这个端口与上面设置端口一样

![](https://gao4.top/wp-content/uploads/2021/02/Snipaste_2021-02-19_17-40-39.png)

图片1

![](https://gao4.top/wp-content/uploads/2021/02/Snipaste_2021-02-19_17-40-50.png)

图片2

最后一步步骤最最最重要的步骤一定需要做的 重启接口 重启接口 重启接口
-----------------------------------

最后一步步骤最最最重要的步骤一定需要做的 重启接口 重启接口 重启接口
-----------------------------------

最后一步步骤最最最重要的步骤一定需要做的 重启接口 重启接口 重启接口
-----------------------------------

最后一步步骤最最最重要的步骤一定需要做的 重启接口 重启接口 重启接口
-----------------------------------

先点关闭再点连接

![](https://gao4.top/wp-content/uploads/2021/02/Snipaste_2021-02-19_17-45-05.png)

客户端
---

另一端就好弄了，直接可以导入配置文件下面是一个典型的例子

```
[Interface]
Address =  
ListenPort =  
PrivateKey =  
DNS =  

[Peer]
AllowedIPs =  
Endpoint =  
PublicKey =  
PersistentKeepalive =  25
```

### 1.1 \[Interface\] 部分介绍

1.  Address：设置虚拟网卡的内网地址（可选子网掩码），填写规则：
    *   可以填写任何符合规范（内网地址可选范围见链接 1,2）的内网地址，但要保证不与虚拟局域网内其它电脑的内网地址相同；
    *   可以写两行；（可选）可以写成自己的IPV6地址： `Address = fd86:ea04:1115::1/64` 。
2.  ListenPort：设置 udp 监听端口，可选范围为 49152 到 65535 。
3.  PrivateKey：填写本机的私钥，默认存储在本机的 `/etc/wireguard/private.key` 文本中。
4.  PostUp：`wg-quick up wg0` 启动后执行的内核防火墙（ iptables ）规则，可以打通 VPN ，服务器端需此参数。
5.  PostDown：`wg-quick down wg0` 执行删除启动时定义的内核防火墙（ iptables ）规则 ，服务器端需此参数。
6.  DNS ，设置 DNS ，不正确设置客户端浏览器网页会无法访问外网地址。
7.  SaveConfig：设为 true 之后，每次重启服务（stop service时）都会自动保存 config 。
8.  MTU：一般不用改，1500没问题

### [](https://github.com/wgredlong/WireGuard/blob/master/2.%E7%94%A8%20wg-quick%20%E8%B0%83%E7%94%A8%20wg0.conf%20%E7%AE%A1%E7%90%86%20WireGuard.md#12-peer-%E9%83%A8%E5%88%86%E4%BB%8B%E7%BB%8D)1.2 \[Peer\] 部分介绍

1.  PublicKey ：连接来节点的公钥，默认存储在其它电脑的 `/etc/wireguard/public.key` 文本中。
2.  AllowedIPs：允许连接的内网 ip 地址。
    *   服务器与客户端应该在同一网段，如客户端的IP为 10.0.2.1/24 ，那么这里可以设置为 10.0.2.0/24 ；
    *   可以写多个，用逗号隔开。
    *   如果写为 0.0.0.0/0 表示允许任何节点连接。
3.  Endpoint ：节点的外网 IP 及端口号，服务器端不需要填写。
4.  PersistentKeepalive：用来保持连接检查的，每过25s会自动检查连通性，

虚拟网络的启动与关闭
----------

启动虚拟网络执行 `wg-quick up wg0`

关闭虚拟网络执行 `wg-quick down wg0`
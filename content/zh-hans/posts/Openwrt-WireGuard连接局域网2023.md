---
title: "Openwrt WireGuard连接局域网2023"
date: 2023-06-09T11:10:08+08:00
slug: 772
draft: false
---

## 搭建环境

 1. 360 T7硬件主路由
 2. ImmortalWrt 21.02
 3. IPV4公网
 4. WireGuard客户端window版本
 5. wg预分配网段192.168.105.1/24
 6. 路由器网段192.168.100.1/24
## 过程
特殊约定后面我把WireGuard统称为wg
#### 创建密钥
在window系统上安装客户端并新建两个隧道
![WireGuard客户端window版本][1]
#### 创建接口
在网络-接口-添加新接口、创建一个名称为WG1协议为WireGuard VPN的接口
![创建新接口界面][2]
去刚刚在客户端里创建的Openwrt隧道私钥 复制到这边创建的WG1接口私钥
![私钥][3]
![私钥2][4]
设置ip地址为`192.168.105.1/24`端口为`5555`,这是可自定义的可按照您需求更改
![设置ip与端口][5]
设置防火墙区域为lan区域
![防火墙区域][6]
#### 设置对端
添加对端-设置一个名称-复制刚刚客户端创建的Window10隧道公钥到接口
![window10隧道公钥][7]
![复制到OP][8]
##### 设置允许iP非常重要
例子如我客户端远程iP为192.168.105.2/24就需要设置为192.168.105.2/32 设置持续 Keep-Alive为`25` 其他可选设置
![允许的 IP][9]
是必须操作的选项每次修改完接口配置都建议重启接口
然后保存并应用-点击重启接口-点击重启接口-点击重启接口
![点击重启接口][10]
##### 再增加一个对端（可选）
对端IP为192.168.105.3/24 需要设置允许IP为192.168.105.3/32
![公钥][11]
设置如下
![第二个对端][12]

#### 设置防火墙
点击网络-防火墙-找到WAN区域-把入站与出站都设置允许
![防火墙区域设置][13]
找到通信规则-添加规则设置需的端口5555
![设置如图][14]
点击保存应用
#### 客户端设置
需要了解PrivateKey为私钥PublicKey为公钥
把两个隧道的公钥交换 并设置好AllowedIPs 一个为WG的网段一个为主路由OP的网段
```
[Interface]
PrivateKey = sBD2AcsmGNddmKRnZItL31lYydshHmxd5ZdVifi5Um0=
Address = 192.168.105.2/24
DNS = 192.168.100.1

[Peer]
PublicKey = 5ZOelbDMJkeXjGuLsVQze8sul+ZnAlYUmAVpsT//hhE=
AllowedIPs = 192.168.104.1/32, 192.168.100.0/24
Endpoint = 171.215.223.90:5555
PersistentKeepalive = 25
```

## 最后验证
点击连接可以看到有流量就知道通了
![点击链接][15]
OP界面也有显示连接
![OP界面显示][16]
ping测试OP与下面局域网设备也可以通过
![ping测试][17]
以上操作都是远程家里OP撰写的，如有问题请留言


  [1]: https://gao4.top/wp-content/uploads/2023/05/2446397780.png
  [2]: https://gao4.top/wp-content/uploads/2023/05/3334229737.png
  [3]: https://gao4.top/wp-content/uploads/2023/05/1562060062.png
  [4]: https://gao4.top/wp-content/uploads/2023/05/1228194948.png
  [5]: https://gao4.top/wp-content/uploads/2023/05/992398305.png
  [6]: https://gao4.top/wp-content/uploads/2023/05/1525696044.png
  [7]: https://gao4.top/wp-content/uploads/2023/05/1102602785.png
  [8]: https://gao4.top/wp-content/uploads/2023/05/3600423415.png
  [9]: https://gao4.top/wp-content/uploads/2023/05/2338709599.png
  [10]: https://gao4.top/wp-content/uploads/2023/05/2406026720.png
  [11]: https://gao4.top/wp-content/uploads/2023/05/3005810838.png
  [12]: https://gao4.top/wp-content/uploads/2023/05/3046932207.png
  [13]: https://gao4.top/wp-content/uploads/2023/05/278395329.png
  [14]: https://gao4.top/wp-content/uploads/2023/05/3875804701.png
  [15]: https://gao4.top/wp-content/uploads/2023/05/824864124.png
  [16]: https://gao4.top/wp-content/uploads/2023/05/1412178774.png
  [17]: https://gao4.top/wp-content/uploads/2023/05/3599056984.png
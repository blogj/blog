---
title: 'Linux 下resilio同步服务安装'
date: Mon, 07 Dec 2020 12:42:46 +0000
draft: false
slug: 325 
tags: ['Arch', 'NAS物语', 'resilio', '原创教程']
---

参考常见问题
------

2021年6月15日更新

网络传输速度测试一直不能满速跑，断断续续的传输，速度一直跑不上来，已弃坑

远程服务器安装后需要修改配置文件把127.0.0.1改成0.0.0.0，不然的话无论是rpm包安装还是仓库安装方式都会访问不了，

2021年5月4日更新 docker镜像网络问题

经测试linuxserver/resilio-sync的安装方式如果网络是以端口转发的方式会对传输有影响，具体是走桥接中继传输数据

![](https://gao4.top/wp-content/uploads/2021/05/image.png)

解决办法是赋予容器一个独立ip或者走当前本地网络，下面是我的docker-compose.yml文件路径修改一下

```
version: "2"
services:
  resilio-sync:
    image: ghcr.io/linuxserver/resilio-sync
    container_name: resilio-sync
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Shanghai
    volumes:
      - /root/resilio/config:/config
      - //mnt/sda/resilio/downloads/:/downloads
      - /mnt/sda/resilio/Sync/:/sync
    network_mode: "host"
    restart: unless-stopped
```

[docker-compose](https://gao4.top/wp-content/uploads/2020/12/baedb53e845ae71.zip)[下载](https://gao4.top/wp-content/uploads/2020/12/baedb53e845ae71.zip)

2021年1月29日更新

前面权限问题可以用下面方法解决，这个问题其实是白名单问题如果遇到下面问题就在配置文件里修改就行unraid docker镜像还是Linux下的问题都可以解决

![](https://gao4.top/wp-content/uploads/2021/01/Snipaste_2021-01-29_15-15-24-1024x514.png)

### 权限问题

安装后有一个权限问题，最快解决办法

运行下面命令找到rslsync.service这个配置文件

```
systemctl status rslsync.service
```

![](https://gao4.top/wp-content/uploads/2020/12/Snipaste_2020-12-07_12-22-51.png)

编辑rslsync.service配置文件

```
sudo vim /usr/lib/systemd/system/rslsync.service
```

修改如下

![](https://gao4.top/wp-content/uploads/2020/12/Snipaste_2020-12-07_12-26-01.png)

重新启动服务

```
sudo systemctl restart rslsync.service
```

#### 后续

猜测是系统启动了rslsync而不是用户启动这个服务，有条件的可以试试反正解决了问题，猜测来源如下图

![](https://gao4.top/wp-content/uploads/2020/12/Snipaste_2020-12-07_12-29-26.png)

安装rslsync服务
-----------

arch

```
yay -S rslsync
``````
systemctl enable rslsync
systemctl start rslsync
```

对于网络环境不好的时候，可以考虑使用离线安装包的方式来安装。官方给了两个包，分别是deb和rpm。

DEB：

```
sudo dpkg -i <resilio-sync.deb>
```

RPM

```
sudo rpm -i <resilio-sync.rpm>
```

分享证书
----

[ResilioSyncPro](https://gao4.top/wp-content/uploads/2020/12/bf547f3296c382a.zip)[下载](https://gao4.top/wp-content/uploads/2020/12/bf547f3296c382a.zip)

### 分享常用安装包

  
[resilio-sync\_x64.tar.gz](https://gao4.coding.net/s/5d0142df-a390-4f43-a5f2-22157fb37aea)

[Resilio-Sync\_x64.exe](https://gao4.coding.net/s/c61b1e03-7c38-46a3-b7a9-5f30ab3ebf7a)

[resilio-sync\_2.6.4.apk](https://gao4.coding.net/s/7e6089f1-5093-4db0-bced-77d15f17f465)

[resilio-sync\_arm.tar.gz](https://gao4.coding.net/s/b3e1eab4-9159-4b64-a2df-1fdcf9940381)

[Resilio-Sync.dmg](https://gao4.coding.net/s/7e6de846-1597-4afe-9013-189d702ee054)

#### 在命令行下载上面相关安装包

![](https://gao4.top/wp-content/uploads/2020/12/Snipaste_2020-12-16_12-07-43.png)

查看链接

```
wget https://gao4.coding.net/s/5d0142df-a390-4f43-a5f2-22157fb37aea -O resilio-sync_x64.tar.gz
```

![](https://gao4.top/wp-content/uploads/2020/12/Snipaste_2020-12-16_12-10-16.png)

wget 下载例子
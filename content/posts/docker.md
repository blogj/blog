---
title: 'docker镜像容器'
date: Sun, 13 Sep 2020 13:18:00 +0000
draft: false
---

公网环境脚本安装docker
--------------

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

国内阿里云镜像加速
---------

```
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://0z9mn9x7.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
systemctl enable docker
```

* * *

**镜像**
------

1\. filebrowser文件管理镜像
---------------------

```
 docker run -d \
         --restart=always \
         --device=/dev/dri/renderD128:/dev/dri/renderD128 \
         -e PUID=0 \
         -e PGID=0 \
         -e WEB_PORT=8083 \
         -e FB_AUTH_SERVER_ADDR=0.0.0.0 \
         -e AUTOCERT_DOMAIN=f.gao4.top \
         -p 8083:8083 \
         -v /mnt/ns/filebrowser/fb:/config \
         -v /mnt/ns/filebrowser/shuju:/myfiles \
        80x86/filebrowser:latest
```

*   [镜像地址](https://hub.docker.com/r/80x86/filebrowser)
*   \--restart=always # 开机启动容器
*   \--device # 把硬解驱动加入容器
*   \-e PUID=0 # 权限设置 0为root权限
*   \-e WEB\_PORT # web访问端口设置
*   \-e FB\_AUTH\_SERVER\_ADDR # 监听地址
*   \-e AUTOCERT\_DOMAIN=f.gao4.top # 外网访问必要参数
*   \-p 8083:8083 # 端口映射 左边8083为主机端口 : 右边为容器端口
*   \-v /mnt/ns/filebrowser/fb:/config # 配置文件目录映射
*   \-v /mnt/ns/filebrowser/shuju # 默认数据存储目录
*   80x86/filebrowser:latest # 镜像

2\. 腾讯DNSPOD-ddns ipv4镜像
------------------------

```
docker run -d \
    --restart=always \
    --name=dnspod-ddns \
    -e "login_token=token_id,token" \
    -e "domain=domain.com" \
    -e "sub_domain=www" \
    -e "interval=10" \
    -e "email=your@email.com" \
    -e "ip_count=1" \
    strahe/dnspod-ddns
```

*   LOGIN\_TOKEN: 必填，在dnspod上申请的api组成的令牌中，参考 [腾讯api获取](https://support.dnspod.cn/Kb/showarticle/tsid/227/)
*   DOMAIN: 必填，在dnspod解析的域名
*   SUB\_DOMAIN: 必填，使用ddns的子域名
*   间隔：选填，进行检查的时间间隔，单位秒，默认为5，建议不要小于5
*   电子邮件：选填，你的邮箱
*   IP\_COUNT：选填，您服务器的出口IP数量，一般为1，填大了一般也没事（玩OpenWrt的可能会有多个IP）

transmission-twc
----------------

```
docker run -d --name=transmission \
--restart=always \
-v /mnt/ns/transmission/config:/config \
-v /mnt/ns/qbittorrent/downloads:/downloads \
-v /mnt/ns/transmission/watch:/watch \
-e PGID=0 -e PUID=0 \
-e TZ=Asia/Shanghai \
--net=host \
oldiy/transmission-twc
```

*   `-v /config` - 配置文件和日志目录
*   `-v /downloads` - 下载目录
*   `-v /watch` - 监视torrent的文件夹
*   `-e PGID` GroupID
*   `-e PUID` UserID
*   `-e TZ` 时区设置

docker qbittorrent
------------------

```
docker run -d \
  --restart=always \
  --name=qbittorrent \
  --net=host \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Aisa/Shanghai \
  -e UMASK_SET=022 \
  -e WEBUI_PORT=8080 \
  -p 8999:8999 \
  -p 8999:8999/udp \
  -p 8080:8080 \
  -v /path/to/appdata/config:/config \
  -v /path/to/downloads:/downloads \
  linuxserver/qbittorrent
```

> 注意 内网ip能访问，外网域名不能访问，去选项-web用户界面-验证 里把"启用主机标头验证"的勾去掉就可以了 默认用户名admin密码adminadmin

![](https://gao4.top/wp-content/uploads/2020/11/11111.png)

ddns-go 推荐使用
------------

```
docker run -d \
  --restart=always \
  --name ddns-go \
  --net=host \
  jeessy/ddns-go
```

Github托管地址

*   自动获得你的公网IPV4或IPV6并解析到域名中
*   支持Mac、Windows、Linux系统，支持ARM、x86架构
*   支持的域名服务商 `Alidns(阿里云)` `Dnspod(腾讯云)` `Cloudflare` `华为云`
*   间隔5分钟同步一次
*   支持多个域名同时解析，公司必备
*   支持多级域名
*   网页中配置，简单又方便，可设置登录用户名和密码
*   网页中方便快速查看最近50条日志，不需要跑docker中查看
*   支持webhook
*   访问主机IP:9876
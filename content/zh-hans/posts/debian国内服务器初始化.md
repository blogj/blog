---
title: 'Debian国内服务器初始化'
date: Thu, 06 Aug 2020 18:56:43 +0000
draft: false
slug: 109 
tags: ['Debian', 'docker', '原创教程']
---

乌班图服务器版本默认ssh居然没有sftp弃之 ， centos虽然很好但总感觉软件包陈旧，燧写这篇初始化操作

更新软件包
-----

```
apt-get update
```

安装阿里云源Docker
------------

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

设置Docker镜像源
-----------

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://0z9mn9x7.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

docker-compose 国内镜像源安装
----------------------

```
curl -L https://get.daocloud.io/docker/compose/releases/download/1.24.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```
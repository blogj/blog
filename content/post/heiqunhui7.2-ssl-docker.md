---
title: "DSM7.2 docker 下申请SSL泛域名证书 并切换切换ZeroSSL"
date: 2024-06-04T23:21:47+08:00
draft: false
slug: 37200
tags: ['群晖', '7.2', 'ssl证书', 'docker部署']
---
# 前言
    通过黑群晖7.2版本docker套件部署ssl证书 且是通过acme.sh切换ZeroSSL申请
### 第一步
在注册表中搜索```neilpang/acme.sh```并下载
![镜像](https://gao4.top/wp-content/uploads/2024/heiqunhui7.2ssl/Snipaste_2024-06-04_23-28-10.png)

### 第二步
启动镜像并填写相关环境
容器名字命名为```acme-sh```和下面第三步脚本执行命令容器名字相同

1. 存储空间


![镜像](https://gao4.top/wp-content/uploads/2024/heiqunhui7.2ssl/Snipaste_2024-06-04_23-31-49.png)


2. 环境变量

```
Ali_Key ： # 填 AccessKey

Ali_Secret ： # 填 AccessKey Secret

SYNO_Username ： # 登录群晖的用户名（建议使用管理员权限）

SYNO_Password ： # 登录群晖的密码

SYNO_Device_ID: #如果你用于登陆的账户启动了二次验证，还需要确定设备ID

SYNO_Certificate ："" # 空字符串（""）为替换默认证书，这里输入任命名来区别于默认证书

SYNO_Create：1 # 表示如果证书不存在，则创建该证书。

SYNO_Port ： # 填入群晖内网的端口号（如果你修改过，默认是5000。）

ACME_EAB_KID: #查看方法，登陆ZeroSSL -> Developer -> ZeroSSL API Key；

ACME_EAB_HMAC_KEY: #查看方法，登陆ZeroSSL -> Developer -> ZeroSSL API Key；
```
![环境变量](https://gao4.top/wp-content/uploads/2024/heiqunhui7.2ssl/Snipaste_2024-06-04_23-41-32.png)

3. 网络类型与执行命令
 网络中勾选使用与 Docker Host 相同的网络
执行命令 -> 在命令栏添加 -> ```daemon```
![环境变量](https://gao4.top/wp-content/uploads/2024/heiqunhui7.2ssl/Snipaste_2024-06-04_23-42-42.png)

### 第三步
撰写脚本Autoupdatecert.sh
路径为/volume1/docker/acme.sh/Autoupdatecert.sh
注意执行脚本前， 容器acme-sh 为运行状态
可通过ssh #模式下执行bash /volume1/docker/acme.sh/Autoupdatecert.sh看是否成功

```
#!/bin/bash

# ==参数说明==
# 1）切换申请商为zerossl
docker exec acme-sh acme.sh --register-account -m i@gao4.top --server zerossl
# 2）申请证书参数
docker exec acme-sh acme.sh --force --log --issue --server zerossl --dns dns_ali --dnssleep 120 -d *.gao4.top
# acme：容器的名字，根据自己的容器名填写
# --server letsencrypt：选的是Let's Encrypt的免费证书
# --dns dns_cf：这里选的是Cloudflare的DNS
# --dnssleep 120：Sleep 120秒
# -d XXX.XXX -d *.XXX.XXX：替换用自己的域名即可，如果有更多的域名，用-d隔开即可。

# 3）部署证书到群晖参数
docker exec acme-sh acme.sh --deploy -d *.gao4.top --deploy-hook synology_dsm

# acme：容器的名字，根据自己的容器名填写
# --deploy -d XXX.XXX -d *.XXX.XXX：对应好自己的之前域名
# --deploy-hook synology_dsm：部署到群晖上
```
### 第四步
在群晖“控制面板”——“任务计划”——“新增”——“计划的任务”——“用户自定义脚本”

常规：给任务明成命名，如“Update SSL”；用户账号选择root。

计划：在以下日期运行，选择一个开始日期，并设置每月重复。

任务设置（关键）：运行命令下输入用户自定义脚本
bash #Autoupdatecert.sh文件路径#
>>#生成日志存放路径# 2>&1

如：

```
bash /volume1/docker/acme.sh/Autoupdatecert.sh >>/volume1/docker/acme.sh/log.txt 2>&1

```
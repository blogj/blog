---
title: "Headscale服务器搭建 20230623验证通过教程"
date: 2023-06-22T20:11:01+08:00
draft: true
---

## 下载二进制文件
```
wget https://ghproxy.com/https://github.com/juanfont/headscale/releases/download/v0.22.3/headscale_0.22.3_linux_amd64
chmod +x headscale_0.22.3_linux_amd64
mv headscale_0.22.3_linux_amd64 /usr/local/bin/headscale
```

## 创建目录并创建数据库
```
mkdir -p /etc/headscale
mkdir -p /var/lib/headscale
mkdir -p /var/run/headscale
touch /var/lib/headscale/db.sqlite
```

## 下载配置文件
```
wget https://ghproxy.com/https://github.com/juanfont/headscale/raw/main/config-example.yaml -O /etc/headscale/config.yaml
```
## 修改配置文件
nano /etc/headscale/config.yaml
```
server_url: http://127.0.0.1:8080
修改为 这选项需要配置域名
server_url: https://he.gao4.top:8080
配置自定义本地证书
tls_cert_path: ""
tls_key_path: ""
修改如下 
tls_cert_path: /root/.acme.sh/he.gao4.top/fullchain.cer
tls_key_path: /root/.acme.sh/he.gao4.top/he.gao4.top.key
配置客户端证书模式
tls_client_auth_mode: relaxed
修改为
tls_client_auth_mode: disabled 
```
## 创建 SystemD service 配置文件
```
nano  /etc/systemd/system/headscale.service 
```
 内容如下
 ```
 [Unit]
Description=headscale
After=network.target

[Service]
WorkingDirectory=/etc/headscale
ExecStart=/usr/local/bin/headscale serve
# Disable debug mode
Environment=GIN_MODE=release

[Install]
WantedBy=multi-user.target 
 ```
 重载SystemD配置文件
 ```
 systemctl daemon-reload 
 ```
## 管理headscale
```
systemctl start headscale.service
systemctl restart headscale.service
systemctl enable headscale.service
systemctl status headscale.service 
```
### 查看是否启动成功
```
ss -tulnp|grep headscale 
systemctl status headscale.service 
```
### 用户管理
可以理解为用户 用户与用户之间是隔离的。如下创建一个为default的用户
```
# 创建命名空间
headscale namespaces create default
# 查看命名空间
headscale namespaces list
ID | Name    | Created
1  | default | 2022-09-02 13:00:37
```
### 密钥认证
给用户default生成密钥
```
headscale preauthkeys create -u default
```
查看密钥
```
headscale -u default preauthkeys list
```
接入客户端
```
tailscale up --login-server=http://<HEADSCALE_PUB_IP>:8080 --accept-routes=true --accept-dns=false --authkey $KEY
```
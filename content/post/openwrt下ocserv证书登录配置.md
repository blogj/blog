---
date: '2025-08-14T12:48:39+08:00'
#draft: true
draft: false
slug: 20250814
title: 'Openwrt下ocserv证书登录配置letsencrypt'
typora-root-url: ../../static
---

## 序言

Openwrt安装OpenConnect VPN配置证书登录与通过Lucky申请ssl证书

## 环境

openwrt 版本

![openwrt](/wp-content/uploads/2025/image-20250814125242089.png)

Lucky版本

![lu环境](/wp-content/uploads/2025/image-20250814125350152.png)

ocserv版本

![image-20250814125516626](/wp-content/uploads/2025/image-20250814125516626.png)

win客户端版本

![image-20250814125544582](/wp-content/uploads/2025/image-20250814125544582.png)

## 配置

### 安装通过ssh上openwrt命令行安装ocserv

```
opkg update
opkg install ocserv luci-app-ocserv
```

### 通过申请的ssl证书去掉下面危险提醒

![危险提醒](/wp-content/uploads/2025/image-20250814130016011.png)

1、去Lucky申请ssl证书后在证书配置最下面有一个证书映射

![证书映射](/wp-content/uploads/2025/image-20250814130108781.png)

2、然后在Openwrt 上OpenConnect VPN里有一个**编辑模版**修改里面下列**配置并保存**为Lucky申请的域名映射里的证书路径

```
server-cert = /etc/ocserv/ssl/gao4范域名.pem
server-key = /etc/ocserv/ssl/gao4范域名.key
```

![image-20250814130459122](/wp-content/uploads/2025/image-20250814130459122.png)

这样就去掉了连接时候证书无效提醒

3、进行测试，开启客户端下面图片选项进行链接

![image-20250814130754110](/wp-content/uploads/2025/image-20250814130754110.png)

4、连接成功，并没有提示证书无效

![连接成功](/wp-content/uploads/2025/image-20250814130914244.png)

### 证书认证登录

用户证书只需 ocserv 信任 CA 即可，因此使用自建 CA 签发证书。

1、创建证书目录并移动到该目录然后通过vi编辑（不会vi编辑建议通过搜索引擎学习一下）

```
mkdir /etc/ocserv/certs/
cd /etc/ocserv/certs/
vi ocm
```

2、粘贴复制下面脚本

```bash
#!/bin/bash

init() {
    WORK="./work"
    CA_TMPL="${WORK}/ca.tmpl"
    CA_KEY="${WORK}/ca-key.pem"
    CA_CERT="./ca.pem"
    USER="$1"
    USER_TMPL="${WORK}/${USER}.tmpl"
    USER_KEY="${WORK}/${USER}-key.pem"
    USER_CERT="${WORK}/${USER}.pem"
    USER_P12="./${USER}.p12"
    REVOKED_CERT="${WORK}/revoked.pem"
    CRL_TMPL="${WORK}/crl.tmpl"
    CRL_CERT="./crl.pem"

    # Ensure working directory
    [[ -d $WORK ]] || mkdir -p $WORK

    # CA Template
    [[ -f $CA_TMPL ]] || cat << _EOF_ > $CA_TMPL
cn = "VPN CA"
serial = 1
expiration_days = 3650
ca
signing_key
cert_signing_key
crl_signing_key
_EOF_

    # CA Private Key
    [[ -f $CA_KEY ]] || certtool --generate-privkey --outfile $CA_KEY

    # CA Certificate
    [[ -f $CA_CERT ]] || certtool --generate-self-signed --load-privkey $CA_KEY --template $CA_TMPL --outfile $CA_CERT
}

generate() {
    # User Template
    cat << _EOF_ > $USER_TMPL
cn = "$USER"
expiration_days = 3650
signing_key
tls_www_client
_EOF_

    # User Private Key
    certtool --generate-privkey --outfile $USER_KEY

    # User Certificate
    certtool --generate-certificate --load-privkey $USER_KEY --load-ca-certificate $CA_CERT --load-ca-privkey $CA_KEY --template $USER_TMPL --outfile $USER_CERT

    # Export User Certificate
    certtool --to-p12 --pkcs-cipher 3des-pkcs12 --load-privkey $USER_KEY --load-certificate $USER_CERT --outfile $USER_P12 --outder
}

revoke() {
    # Copy User Certificate to Revoked Certificate
    cat $USER_CERT >> $REVOKED_CERT

    # CRL Template
    [[ -f $CRL_TMPL ]] || cat << _EOF_ > $CRL_TMPL
crl_next_update = 3650
crl_number = 1
_EOF_

    # CRL Certificate
    certtool --generate-crl --load-certificate $REVOKED_CERT --load-ca-privkey $CA_KEY --load-ca-certificate $CA_CERT --template $CRL_TMPL --outfile $CRL_CERT
}

case $1 in
    generate)
        init $2
        generate
        ;;
    revoke)
        init $2
        revoke
        ;;
    *)
        echo "\
Usage:
    $0 generate USER
    $0 revoke USER
"
esac
```

3、给于脚本权限

```
chmod 775 ocm 
```

4、创建用户命令

```
./ocm generate USERNAME 即可直接生成用户
```

![image-20250814132832956](/wp-content/uploads/2025/image-20250814132832956.png)

5、最重要的OpenConnect VPN配置

在参考模版下进行下面配置

```
auth = "certificate"
#auth = "|AUTH|"
ca-cert = ca-cert = /etc/ocserv/certs/ca.pem
cert-user-oid = 2.5.4.3

```

点击保存并运用

6、**重新启动一下OpenConnect VPN**

去掉勾保存运行，再勾选启动，来对配置文件进行生效，如果没有做这一步，可能你点击保存并运用后会出现网络连接不成功提示

![image-20250814133525409](/wp-content/uploads/2025/image-20250814133525409.png)

### 客户端下载第三方

https://ocserv.yydy.link:2023/#/

## 参考

https://www.ioiox.com/archives/90.html

https://blog.0u0.moe/ocserv-letsencrypt-certificate/

https://www.ioiox.com/archives/89.html

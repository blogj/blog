---
title: '关于openwrt不定时断网研究与解决办法'
date: Tue, 06 Sep 2022 11:55:13 +0000
tags: ['NAS物语', '资料']
slug: 1114
---

## 三种解决断网方法

选择任意一种看能不能解决刷了 openwrt 后光猫桥接 openwrt 当主路由不定时断网问题。

### 第一种（推荐）

`修改LCP 响应故障阈值`与`LCP 响应间隔`

![界面图](https://gao4.top/wp-content/uploads/2023/05/2853494952.png)
LCP 响应故障阈值为 每隔 10 秒发送一次

LCP 响应间隔为 未响应 10 次就断开链接重新拨号

### 第二种 （冗余方案）

撰写脚本来只要断网就重启 WAN 口重新拨号

第一步通过 ssh 登录 openwrt 后台复制下面命令回车执行

```
vi /root/dwjb.sh
```

按`i`进入编辑模式

第二步 复制下面脚本粘贴

```
#!/bin/sh
tries=0
logger "my network watchdog start"
while [[ $tries -lt 5 ]]
do
        if /bin/ping -c 1 114.114.114.114 >/dev/null
        then
            logger "network pass, exit."
            exit 0
        fi
        tries=$((tries+1))
        sleep 10
done
logger "network error, restart network"
/sbin/ifup wan
```

粘贴完成 按【ESC】键跳到命令模式,然后再按【:】冒号键,最后再按【wq】,即可保存退出 vi 的编辑状态;

第三步 在 web 界面定时任务内填入以下内容

```
*/6 * * * * sh /root/dwjb.sh
```

![web界面定时任务](https://gao4.top/wp-content/uploads/2023/05/2196474465.png)

以上代码表示每 6 分钟检查一次网络
通过设置，即可实现路由器监测到断网，自动重启网路并重新拨号。

### 第三种备用方案

通过 Watchcat 插件来检测断网重启接口

这个方案您得基本很了解 openwrt 至少知道怎么安装插件，因为现在第三方固件很多，也有可能您得自己编译固件才行。而且插件感觉不太稳定。
![Watchcat](https://gao4.top/wp-content/uploads/2023/05/2841602605.png)

![Watchcat](https://gao4.top/wp-content/uploads/2023/05/4123531025.png)

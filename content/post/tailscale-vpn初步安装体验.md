---
title: 'Tailscale VPN初步安装体验'
date: Thu, 13 May 2021 16:13:00 +0000
draft: false
slug: 503 
tags: ['NAS', 'NAS物语', 'tailscale', 'VPN', '原创教程']
---

序言
--

初步体验是一个基于[wireguard](http://wireguard.com)的新的星形组网软件（功能之一），对比WGvpn最大的不同就是增加了公匙交换系统，免配置组网，只需要安装客户端登录账号就可以组网，支持p2p打洞，付费自建中继功能。支持基本上全平台支持Linux|群晖|Openrt|Window|Mac等常见客户端，docker也可以安装。

Openwrt X86上安装
--------------

2021年5月13日更新

openwrt 仓库已经有了安装包直接更新一下仓库就可以搜索到如下图

![](https://gao4.top/wp-content/uploads/2021/05/image-1.png)

执行下面命令就可以安装

```
opkg update
opkg install tailscale
```

Ps：[在X86下扩容安装软件](https://www.vediotalk.com/archives/13889)[Overlay](https://www.vediotalk.com/archives/13889)空间的教程亲测可用

Openwrt arm上安装
--------------

```
opkg update
opkg install tailscale
```

PS:注意安装空间的大小，占满空间会保存不了Openwrt配置，一些定时任务失效

[tailscale-synolog](https://github.com/tailscale/tailscale-synology)群晖下安装
-------------------------------------------------------------------------

[在Github上下载合适的版本](https://github.com/tailscale/tailscale-synology/releases)上传到群晖安装即可，然后ssh登录群晖sudo tailscale up 即可

![](https://gao4.top/wp-content/uploads/2021/05/image-2.png)

![](https://gao4.top/wp-content/uploads/2021/05/image-3.png)

Unraid下用docker安装
----------------

在APPS里搜索 然后按照正常启动容器就可以，不用修改什么

![](https://gao4.top/wp-content/uploads/2021/05/image-4-1024x447.png)

然后点击图标双击第一个>\_Console 在启动的命令行窗口输入命令tailscale up 获取登录链接就可以。

![](https://gao4.top/wp-content/uploads/2021/05/image-5.png)

PS：遇到问题
-------

0x1安卓下远程控制0x300005f错误代码

![](https://gao4.top/wp-content/uploads/2021/05/image-6.png)

要修复此问题，请在“PC名称”字段中使用您的IP地址。

0x2 [openwrt 空间不足问题](https://www.vediotalk.com/archives/13889)

![](https://gao4.top/wp-content/uploads/2021/05/image-7.png)

arm重新恢复出厂设置吧

0x3 openwert ssh登录不了问题 直接未指定就行

![](https://gao4.top/wp-content/uploads/2021/05/image-17-1024x316.png)

\### win10升级win11后服务启动失败 解决办法 我可以确认我在从 Win10 升级到 Win11 后遇到了类似的问题，但结合了来自\[@iball\](https://github.com/iball)和\[@DentonGentry 的\](https://github.com/DentonGentry)两种解决方案。 \* 卸载 Tailscale \* 删除 %USERPROFILE%\\AppData\\Local\\Tailscale \* 重新安装尾鳞 现在需要重新登录Tailscale，登录后，Win11 中的Tailscale 效果很赞。
---
title: 'unraid6.9.2安装及UEFI引导与初始化'
date: Sun, 01 Aug 2021 22:07:00 +0000
draft: false
slug: 649 
tags: ['NAS', 'NAS物语', 'Unraid', 'unraid6.9.2', '原创教程']
---

前言
==

系统包来源于俄罗斯论坛

系统盘制作
=====

系统下载
----

分享俄罗斯版本[链接](https://softoroom.org/topic89043.html)

使用官方工具制作

1.  把下载的unRAIDServer-6.9.2-x86\_64\_fu11压缩包解压
2.  运行Unraid.USB.Creator.Win32-1.6

![官方制作工具](https://gao4.top/wp-content/uploads/2021/08/1676644806.png "官方制作工具")

3.  Allow UEFI Boot 勾选 需要UEFI引导一定需要使用官方工具制作 使用`UnraidTool`制作UEFI引导的系统U盘不一定生效
4.  生成密钥，复制`UnraidTool.exe`与`keymaker.exe`到U盘根目录 右击管理员身份运行 UnraidTool 点击第二个`注册` 系统制作完毕  
    ![UnraidTool](https://gao4.top/wp-content/uploads/2021/08/4095878892.png "UnraidTool")

进入系统初始化操作
=========

GitHub访问
--------

```
echo "199.232.4.133 raw.githubusercontent.com" >> /etc/hosts
```

![GitHub访问](https://gao4.top/wp-content/uploads/2021/08/3282416052.png "GitHub访问")

Docker市场
--------

```
https://raw.githubusercontent.com/Squidly271/community.applications/master/plugins/community.applications.plg
```

![安装Docker市场](https://gao4.top/wp-content/uploads/2021/08/3954018899.png "安装Docker市场")

安装中文语言包
-------

在Docker市场点击 **左边** **Language**找到中文语言包安装即可

![中文语言包](https://gao4.top/wp-content/uploads/2021/08/1974993561.png "中文语言包")

![切换中文](https://gao4.top/wp-content/uploads/2021/08/3264224320.png "切换中文")  
选择中文如何点击APPLY确定  
![切换即可](https://gao4.top/wp-content/uploads/2021/08/3824821100.png "切换即可")

设置阵列自启
------

![阵列自启](https://gao4.top/wp-content/uploads/2021/08/2732363111.png "阵列自启")

开启核显
----

`安装CA CONFIG EDITOR`\`一个在线配置文件编辑器\`  
![安装插件](https://gao4.top/wp-content/uploads/2021/01/Snipaste_2021-01-23_20-19-03-1024x324.png "安装插件")  
点击插件  
![点击插件](https://gao4.top/wp-content/uploads/2021/08/626629736.png "点击插件")  
![页面](https://gao4.top/wp-content/uploads/2021/08/1099005542.png "页面")

```
modprobe i915
chown nobody:users /dev/dri
chmod 0777 /dev/dri/*
```

必安装插件
=====

1.  **User Scripts**  
    一款可以运行用户自定义脚本的插件。贴吧有人使用这个插件设置每次开机自动更换docker源。官方解释为：一个插件，可充当任何用户脚本的简单前端，使您无需输入命令行即可运行它们。  
    ![User Scripts](https://gao4.top/wp-content/uploads/2021/08/3505330592.png "User Scripts")
2.  **rclone**  
    类似于rsync但是可以连通所有云服务器的命令行软件, 用它来做远程备份或者下载很方便.  
    ![rclone](https://gao4.top/wp-content/uploads/2021/08/1398285922.png "rclone")
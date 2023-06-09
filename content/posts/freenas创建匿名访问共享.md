---
title: 'freenas创建匿名访问共享'
date: Sat, 08 Aug 2020 08:54:32 +0000
draft: false
slug: 127 
tags: ['freenas', 'NAS物语', '转载学习']
---

一、创建匿名访问共享#1.1 创建
-----------------

进入Sharing ➞ Windows (SMB) Shares,然后点击ADD按钮界面如下图所示勾选Allow Guest Access直接按SAVE保存

![](https://gao4.top/wp-content/uploads/2020/08/FreeNAS15.52.27-1024x367.png)

匿名共享图一

**注意事项** 如果共享创建在根目录，没有任何权限处理，默认是只读的，要进一步控制权限，需要创建二级数据集，并设置权限。权限如下图：

![](https://gao4.top/wp-content/uploads/2020/08/FreeNAS16.22.02-1024x282.png)

匿名共享图二
---
title: 'TrueNAS-SCALE SMB共享设置'
date: Wed, 19 May 2021 01:51:32 +0000
draft: false
slug: 538 
tags: ['NAS', 'NAS物语', 'SMB', 'TrueNAS', '原创教程']
---

#### 1.打开win10 SMB1.0

SMB1.0功能 此电脑>卸载或更改程序>程序和功能>启用或关闭win功能 勾选?

![](https://gao4.top/wp-content/uploads/2021/05/image-8.png)

勾选

#### 2.添加用户

登录TrueNAS 仪表盘>证书>Local Users>添加 添加一个用户user1修改密码，保持默认选项点击保存?

![](https://gao4.top/wp-content/uploads/2021/05/image-10-1024x280.png)

添加用户

#### 3.添加数据集

添加数据集填入名字test1其他保持默认点击保存，存储>阵列旁边三个小点>Add Dataset ?

![](https://gao4.top/wp-content/uploads/2021/05/image-11-1024x430.png)

#### 4.修改数据集权限

点击数据集右边三个小点>点击Edit Permissions 修改用户与群组为user1 勾选下面的Apply User不然修改无效其他默认即可，点击保存?

![](https://gao4.top/wp-content/uploads/2021/05/image-12.png)

数据集权限设置

#### 5.添加SMB共享

添加名为tes1的共享 Shares> Windows Shares(SMB)>添加 φ(\*￣0￣)

![](https://gao4.top/wp-content/uploads/2021/05/image-13-1024x285.png)

选择路径 刚刚添加的数据集路径默认即可，点击保存?

![](https://gao4.top/wp-content/uploads/2021/05/image-14.png)

#### 6.win文件管理器访问smb测试

![](https://gao4.top/wp-content/uploads/2021/05/image-15.png)

**PS：一定需要添加新用户 root用户无法访问，客户端SMB1.0一定需要开启不然无法访问。**?
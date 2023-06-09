---
title: 'openmediavault SMB共享设置'
date: Sat, 08 Aug 2020 13:05:58 +0000
draft: false
slug: 132 
tags: ['NAS物语', 'openmediavault', '转载学习']
---

创建用户
----

默认的admin用户与root用户权限都设置完毕也不能登陆smb，创建一个用户就正常了

访问权限管理-用户-添加用户

![](https://gao4.top/wp-content/uploads/2020/08/IMG_20200808_124702.jpg)

添加用户

创建文件系统
------

![](https://gao4.top/wp-content/uploads/2020/08/IMG_20200808_125504-1024x641.jpg)

创建文件系统ext4

挂载硬盘
----

![](https://gao4.top/wp-content/uploads/2020/08/IMG_20200808_125156-1024x319.jpg)

硬盘挂载

创建共享文件夹
-------

![](https://gao4.top/wp-content/uploads/2020/08/OMV-SMB5-1024x677-1.png)

访问权限管理 —-> 共享文件夹点击添加根据自己需求设置名称，路径它自己会自动创建点击保存如需多个共享文件夹，按照上面依次设置即可

开启smb服务
-------

服务 —> SMB/CIFS启动服务 —> 保存

![](https://gao4.top/wp-content/uploads/2020/08/OMV-SMB6-1024x678-1.png)

添加smb共享文件夹
----------

![](https://gao4.top/wp-content/uploads/2020/08/OMV-SMB7-1024x679-1.png)

然后切换到共享，

选择所需添加的共享文件夹点击保存

然后把需要开启SMB的文件夹依次添加进去
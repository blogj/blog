---
title: 'Arch 安装sshd问题'
date: Tue, 08 Sep 2020 08:35:26 +0000
draft: false
slug: 196 
tags: ['Arch', 'linux', '资料', '转载学习']
---

启动ssh
-----

```
systemctl restart sshd
```

开机自启ssh
-------

```
systemctl enable ssh
```

设置root用户远程登陆
------------

arch 远程ssh默认是禁止登陆root的

```
nano /etc/ssh/sshd.config
# 添加下面一行配置
PermitRootLogin yes
```

nano 文本编辑器使用
------------

### 退出

> 按Ctrl+X

> 如果你修改了文件，下面会询问你是否需要保存修改。输入Y确认保存，输入N不保存，按Ctrl+C取消返回。

> 如果输入了Y，下一步会让你输入想要保存的文件名。如果不需要修改文件名直接回车就行；若想要保存成别的名字（也就是另存为）则输入新名称然后确 定。这个时候也可用Ctrl+C来取消返回。
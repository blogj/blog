---
title: 'Croc'
date: Sun, 21 Nov 2021 12:54:00 +0000
draft: false
---

croc 是一款 Go 语言便携的命令行工具，它能够轻松、安全地传输文件，内置一个公共中继服务器，可以实现内网穿透，也支持自建中继服务器。@[Appinn](https://www.appinn.com/croc/)

![ title=](https://img3.appinn.net/images/202110/croc.jpg!o " title=")

croc 适合于喜欢用命令行的同学，当然也适合初学者，不需要配置复杂的参数，直接用就行：

### 发送文件

```
croc send 文件路径
```

会自动返回接收代码

### 接收文件

```
croc 接收代码
```

![ title=](https://img3.appinn.net/images/202110/screen-appinn2021-10-27_16_01_16.jpg!o " title=")

如何安装 croc
---------

它可以通过 Homebrew、Scoop、[Chocolatey](https://www.appinn.com/chocolatey/)、Nix 等软件包管理工具安装：

对于 macOS 用户，请先安装 brew：

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

对于 Windows 用户，请使用 PowerShell 安装 scoop：

```
iwr -useb get.scoop.sh | iex
```

然后就可以安装 croc 了：

```
// macOS
brew install croc 

// Windows
scoop install croc 
choco install croc

// Linux
curl https://getcroc.schollz.com | bash
```
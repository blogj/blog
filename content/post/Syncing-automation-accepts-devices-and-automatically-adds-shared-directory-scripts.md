---
title: "Syncthing自动化接受设备与自动添加共享目录脚本"
date: 2024-06-21T09:22:44+08:00
draft: false
slug: 20240621
tags: ['Syncthing', '自动化', '脚本', 'P2P共享']
---
## 序言

在当前2024/6/21参考官方论坛与github官方讨论，开发者不愿意开发自动接受设备与连接设备后自动添加共享目录，官方更愿意让使用者自己通过api构建程序脚本来增强相关功能。

## 脚本

```
nano sync.sh
```

```
#!/bin/bash

api_key="YOUR_API_KEY_HERE"
syncthing_url="http://localhost:8384"

while :
do
    device_id=$(curl -s -X GET -H "X-API-Key: ${api_key}" "${syncthing_url}/rest/cluster/pending/devices" | jq -r "keys[0]" )
    time=$(date "+%Y-%m-%d %H:%M:%S")
    if [ "$device_id" = "null" ] || [ ${#device_id} -lt 5 ]; then
        echo $time "未找到待连接的设备"
    else
        curl -s -X PUT -H "X-API-Key: ${api_key}" -d "[{\"deviceID\": \"$device_id\"}]" ${syncthing_url}/rest/config/devices
        curl -s -X PATCH -H "X-API-Key: ${api_key}" -d "{ "\devices\":"[{\"deviceID\": \"$device_id\"}]}" ${syncthing_url}/rest/config/folders/9ecev-bio4v
        echo $time "$device_id 连接成功并添加共享文件夹"
    fi
    sleep 30 
done
```

## 注意事项(必看)
1. YOUR_API_KEY_HERE 在右上角 操作-设置-常规-API 密钥找到
2. jq命令必须安装这是解析设备id用的常规Linux发行版本可能没有带

### 开机启动脚本
因为每个发行版本可能不一样就偷懒不写了q.q

## 参考
1. Syncthing API 示例 https://forum.syncthing.net/t/syncthing-api-examples/22123123

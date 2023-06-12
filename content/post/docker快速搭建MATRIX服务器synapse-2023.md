---
title: 'docker快速搭建MATRIX服务器synapse-2023'
date: Sun, 11 Sep 2023 13:31:00 +0000
draft: false
slug: 227
tags: ['NAS物语', 'MATRIX', 'VPN', '原创教程']
---


docker 快速搭建 MATRIX 服务器 synapse

## 环境

- debian11
- docker
- synapse:v1.83.0
- postgres:12-alpine
- 域名一个 不能轻易修改
- ipv6 地址

## 安装

新建一个文件夹建立 yml 文件`nano docker-compose.yml`内容如下注意自己 生成数据库密码

```
#也感谢糖喵提供的配置文件~
version: "3.4"

services:
  synapse:
    hostname: matrix
    image: matrixdotorg/synapse:v1.83.0
    restart: always
    container_name: matrix_server
    depends_on:
      - db
      - redis
    ports:
      - "127.0.0.1:8001:8008"
    volumes:
      - ./synapse/data:/data
    networks:
      - synapse_network
      - external_network
    healthcheck:
      test: ["CMD-SHELL", "curl -s localhost:8008/health || exit 1"]

  db:
    image: postgres:12-alpine
    restart: always
    container_name: matrix_db
    volumes:
      - ./synapse/db:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: synapse
      POSTGRES_PASSWORD: 配置数据库密码
      POSTGRES_DB: synapse
      POSTGRES_INITDB_ARGS: "--encoding='UTF8' --lc-collate='C' --lc-ctype='C'"
    networks:
      - synapse_network
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "synapse"]

  redis:
    image: redis:6.0-alpine
    restart: always
    container_name: matrix_redis
    volumes:
      - ./synapse/redis:/data
    networks:
      - synapse_network
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]

networks:
  synapse_network:
    internal: true
  external_network:
```

运行如下内容进行生成配置文件

```
docker-compose run --rm -e SYNAPSE_SERVER_NAME=服务器域名地址 -e SYNAPSE_REPORT_STATS=no synapse generate
```

注意生成的服务器域名地址官方不建议修改，后面也不能修改不然会报错启动不了服务器

## 配置

编辑配置文件 `nano ./synapse/data/homeserver.yaml`
在内容中找到 sqlite3 相关的配置，然后注释掉或者删去。参考替换为以下内容：

```
server_name: "myvessel.top"
pid_file: /etc/matrix-synapse/homeserver.pid
listeners:
  - port: 8008
    tls: false
    type: http
    x_forwarded: true
    resources:
      - names: [client, federation]
        compress: false
database:
  name: psycopg2
  txn_limit: 10000
  args:
    user: synapse
    password: 数据库密码
    database: synapse
    host: db
    port: 5432
    cp_min: 5
    cp_max: 10
#database:
#  name: sqlite3
#  args:
#    database: /data/homeserver.db
#↑注释掉使用 sqlite3 的配置

redis:
  # Uncomment the below to enable Redis support.
  #
  enabled: true

  # Optional host and port to use to connect to redis. Defaults to
  # localhost and 6379
  #
  host: redis
  port: 6379

suppress_key_server_warning: true
enable_registration: true
enable_registration_without_verification: true
```

trusted_key_servers 是现有的信任服务器，不能为空，默认为 matrix.org，可以加入自己认识的实例，我这里添加了 neo.angry.im 和 bgme.me 虽然并没有用上。除此之外最后 3 行是我新配置的内容，效果分别如下：

- suppress_key_server_warning：如果 trusted_key_servers 中有 matrix.org，此项须设为 true，否则启动时会有警告。
- enable_registration：设为 true 时允许注册。不能单独启用，下面必须设置注册验证方式或设为不验证即可注册。详情请查阅官方说明。
- enable_registration_without_verification：设为 true 时允许不验证即可注册。

### 启动

```
docker-compose up -d
```

### nginx 反向代理设置

新建一个`synapse.conf`

```
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    server_name synapse.matrix.org;

    ssl_certificate /etc/letsencrypt/live/synapse.matrix.org/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/synapse.matrix.org/privkey.pem;

    # Various TLS hardening settings
    # https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;
    ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384;
    ssl_session_timeout  10m;
    ssl_session_cache shared:SSL:10m;
    ssl_session_tickets on;
    ssl_stapling on;
    ssl_stapling_verify on;


    location ~ ^(/_matrix|/_synapse/client) {
        # note: do not add a path (even a single /) after the port in `proxy_pass`,
        # otherwise nginx will canonicalise the URI and cause signature verification
        # errors.
        proxy_pass http://127.0.0.1:8001;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $host;

        # Nginx by default only allows file uploads up to 1M in size
        # Increase client_max_body_size to match max_upload_size defined in homeserver.yaml
        client_max_body_size 500M;
    }

}
```

- 重载 nginx 配置文件，nginx -s reload
- 创建注册新用户

```
docker-compose exec  synapse register_new_matrix_user -c /data/homeserver.yaml http://localhost:8008
```

- 完成后用任意一个客户端登陆即可使用，注意登陆用的地址是你规划的域名地址不能轻易修改

## 参考教程
[搭建 Matrix 的服务端 Synapse][1]
[搭建MATRIX即时通信服务][2]


  [1]: https://b.myvessel.top/archives/deploy-synapse
  [2]: https://blog.southfox.me/2022/04/%E6%90%AD%E5%BB%BAMatrix%E5%8D%B3%E6%97%B6%E9%80%9A%E4%BF%A1%E6%9C%8D%E5%8A%A1/
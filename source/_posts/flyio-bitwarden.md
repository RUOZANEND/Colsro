---
title: 使用fly.io搭建Bitwarden
categories: 
  - Tech
tags:
  - Docker
  - Flyio
date: 2021-03-14 09:42:00
updated: 2021-03-14 09:42:00
thumbnail: /imgs/flyio-bitwarden/cover.jpg
pslug: flyio-bitwarden
---
Bitwarden 是一款开源的密码管理器,支持 Web、Chrome 浏览器插件,拥有 iOS、Android 客户端,采用本地加密,云同步的方式

<!--more-->

下面开始教程

首先你需要一个fly.io账号，如果没有可以参考： https://jingyuan.wang/p/fly-io 安装使用
然后准备一个MySQL数据库。如果没有可以使用db4free提供的免费MySQL数据库
接下来依次执行：
```bash
mkdir bitwarden && cd bitwarden
```
随后修改下面的关键信息后复制粘贴执行：
```bash
cat >fly.toml <<EOF
app = "your_flyioapps_name"

[build]
  image = "bitwardenrs/server"

[[services]]
    internal_port = 80
    protocol = "tcp"

    [[services.ports]]
      handlers = ["http"]
      port = "80"

    [[services.ports]]
      handlers = ["tls", "http"]
      port = "443"

[env]

RUST_BACKTRACE = "1"
DATABASE_URL = "mysql://database_user_name:database_password@database_host/database_name"
ADMIN_TOKEN = "your_admin_password"
ENABLE_DB_WAL = "false"
EOF
```

最后执行`fly deploy`命令部署即可

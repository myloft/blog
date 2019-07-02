---
title: v2ray h2 + web 配置
date: 2019-02-15 16:42:21
updated: 2019-02-15 16:42:21
tags: 网络
---
　　v2ray的h2+web模式要比websocket+tls+web模式的配置复杂一点，性能也相对好一点，记录一下配置。
<!-- more --> 

Caddy配置
---
```
example.com {
    root www
    proxy /ray https://localhost:10000 {
        insecure_skip_verify
        transparent
    }
}
```

主页配置
---
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to caddy!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to caddy!</h1>
<p>If you see this page, the caddy web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://caddyserver.com/">caddyserver.com</a>.

<p><em>Thank you for using caddy.</em></p>
</body>
</html>
```
服务端配置
---
```json
{
  "inbounds": [
    {
      "port": 10000,
      "listen": "127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "uuid",
            "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "h2",
        "httpSettings": {
          "path": "/ray",
          "host": ["example.com"]
        },
        "security": "tls",
        "tlsSettings": {
          "certificates": [
            {
              "certificateFile": "/etc/v2ray/v2ray.crt",
              "keyFile": "/etc/v2ray/v2ray.key"
            }
          ]
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

客户端配置
---
```json
{
  "inbounds": [
    {
      "port": 1080,
      "listen": "127.0.0.1",
      "protocol": "socks",
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      },
      "settings": {
        "auth": "noauth",
        "udp": true
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "example.com",
            "port": 443,
            "users": [
              {
                "id": "uuid",
                "alterId": 64
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "h2",
        "httpSettings": {
          "path": "/ray",
          "host": ["example.com"]
        },
        "security": "tls"
      }
    }
  ]
}
```
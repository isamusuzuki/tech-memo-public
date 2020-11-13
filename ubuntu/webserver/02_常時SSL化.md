# 常時 SSL 化

作成日 2020/02/02

## 01. Nginx の設定例

```text
server {
   listen 80 default_server;
   server_name   mattermost.eg-wms.com ;
   return 301 https://$server_name$request_uri;
}

server {
  listen 443 ssl http2;
  server_name    mattermost.eg-wms.com ;

  ssl on;
  ssl_certificate /etc/letsencrypt/live/mattermost.eg-wms.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/mattermost.eg-wms.com/privkey.pem;
  ssl_session_timeout 1d;
  ssl_protocols TLSv1.2;
  ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
  ssl_prefer_server_ciphers on;
  ssl_session_cache shared:SSL:50m;
}
```

## 02. HSTS とは

HSTS = HTTP Strict Transport Security

HTTP で接続した際に、強制的に HTTPS へリダイレクトし、以降のそのドメインへの接続はすべて HTTPS とする機能

以下を、HTTP ヘッダに含むことで実現される

```text
Strict-Transport-Security:max-age=31536000;includeSubDomains
```

31536000 = 1 年間の秒数

## 03. Preloaded HSTS とは

ブラウザ本体に HSTS で接続するドメインのリストがある

Chrome の Preloaded HSTS に組み入れるためは、以下のサイトで登録する

[HSTS Preload List Submission](https://hstspreload.org/)

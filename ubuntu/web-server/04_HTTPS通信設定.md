# HTTPS 通信設定

作成日 2023/04/10、更新日 2023/04/21

## 01. Certbot のインストール

[Certbot Instructions | Certbot](https://certbot.eff.org/instructions)

```bash
sudo snap install --classic certbot
```

## 02. certbot の実行

```bash
sudo certbot --nginx
# => Enter email address: 自分のメルアドを入力する
# => Please read the Terms of Service: チェックを入れる
# => Would you share your email: 了解する
# => Which names for HTTPS: avocado.example.comが候補に登場するのでそれを選ぶ

# 自動的にSSL証明書が更新されるか、ドライランを行う
sudo certbot renew --dry-run
```

Certbot は Nginx の設定ファイル (`/etc/nginx/sites-available/avocado`) を更新する

ブラウザで `https://avocado.example.com` にアクセスすると、JSON 文字列が表示される

## 03. Nginx 設定ファイルの確認

/etc/nginx/sites-available/avocado

```bash
# 以下は、https通信の設定
server {
  listen [::]:443 ssl ipv6only=on;
  listen 443 ssl;
  server_name avocado.example.com;
  # 以下は、certbot が書いたコード
  ssl_certificate /etc/letsencrypt/live/avocado.example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/avocado.example.com/privkey.pem;
  include /etc/letsencrypt/options-ssl-nginx.conf;
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

  # 以下は、Gunicornへの転送
  location / {
    include proxy_params;
    proxy_pass http://unix:/home/ubuntu/avocado/avocado.sock;
  }
}

# 以下は、http通信の設定
server {
    if ($host = avocado.example.com) {
        return 301 https://$host$request_uri; # https通信に転送する
    }
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name avocado.example.com;
    return 404;
}
```

# Nginx 設定

作成日 2021/01/30

## 01. 目的

- OS ... Ubuntu 20.04 LTS
- 言語 ... Python 3.8
- Web フレームワーク ... Flask
- WSGI サーバー ... Gunicorn
- リバースプロキシーサーバー ... Nginx

上記の構成で、インターネット上に Web サーバーを構築する

Let's Encrypt の無償の SSL 証明書を使って、HTTPS 通信を達成する

### 前提条件

サーバーには `avocado.example.com` というホスト名がすでに設定されているものとする

## 02. Nginx のインストール

```bash
sudo apt install nginx

nginx -v
# => nginx version: nginx/1.18.0 (Ubuntu)
```

ブラウザで `http://avocado.example.com` にアクセスすると、Nginx のデフォルトページが表示される

### Nginx 設定ファイルの編集 1 回目

Certbot (Let's Encrypt のツール) がスムーズに進行するように、最低限の編集を行う

```bash
sudo vi /etc/nginx/sites-available/default
# => server_name を編集する
```

/etc/nginx/sites-available/default

```text
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    # server_name _;                  // この行をコメントアウトする
    server_name avocado.example.com;  // この行を追加する
}
```

## 03. Certbot のインストール

[Certbot \- Ubuntufocal Nginx](https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx)

```bash
sudo snap install --classic certbot
```

### certbot の実行

```bash
sudo certbot --nginx
# => Enter email address: 自分のメルアドを入力する
# => Please read the Terms of Service: チェックを入れる
# => Would you share your email: 了解する
# => Which names for HTTPS: avocado.example.comを入力する

# 自動的にSSL証明書が更新されるか、ドライランを行う
sudo certbot renew --dry-run
```

Certbot は Nginx の設定ファイル (`/etc/nginx/sites-available/default`) を更新する

ブラウザで `https://avocado.example.com` にアクセスすると、Nginx のデフォルトページが表示される

### Nginx 設定ファイルの編集 2 回目

/etc/nginx/sites-available/avocado

```text
// http通信は、こちらの設定
server {
  listen 80 default_server;
  listen [::]:80 default_server;
  server_name avocado.example.com;
  // 以下を追加する
  return 301 https://$host$request_uri;  // https通信に転送する
}

// https通信は、こちらの設定
server {
  listen [::]:443 ssl ipv6only=on;
  listen 443 ssl;
  server_name avocado.example.com;
  // 以下は、certbot が書いたコードをそのままコピペする
  ssl_certificate /etc/letsencrypt/live/avocado.example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/avocado.example.com/privkey.pem;
  include /etc/letsencrypt/options-ssl-nginx.conf;
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

  // 以下は、Gunicornへの転送を行う
  location / {
    include proxy_params;
    proxy_pass http://unix:/home/ubuntu/avocado/avocado.sock;
  }
}
```

Nginx の設定を切り替えて、再起動する

```bash
# リンクファイルを作り直すことで、設定を切り替える
sudo rm /etc/nginx/sites-enabled/default
sudo ln -s /etc/nginx/sites-available/avocado /etc/nginx/sites-enabled/default

# シンタックスエラーをチェックする
sudo nginx -t

# Nginxを再起動する
sudo systemctl restart nginx
```

ブラウザで `https://avocado.example.com` にアクセスすると、JSON 文字列が表示される

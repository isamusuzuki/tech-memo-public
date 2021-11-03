# Nginx 設定

作成日 2021/11/03

## 01. 目的

下記の構成で、インターネット上に Web サーバーを構築する

- OS ... Ubuntu 20.04 LTS
- Webサーバー ... Nginx

Let's Encrypt の SSL 証明書（無料）を使って、HTTPS 通信を達成する

### 前提条件

DNSサーバーには `avocado.example.com` というホスト名がすでに登録されているものとする

## 02. Nginx のインストール

```bash
sudo apt install nginx

nginx -v
# => nginx version: nginx/1.18.0 (Ubuntu)
```

ブラウザで `http://avocado.example.com` にアクセスすると、Nginx のデフォルトページが表示される

### Nginx 設定ファイルの編集

Certbot (Let's Encrypt のツール) がスムーズに進行するように、最低限の編集を行う

```bash
sudo vi /etc/nginx/sites-available/default
# => server_name を編集する
```

/etc/nginx/sites-available/default

```bash
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    # server_name _;                  # この行をコメントアウトする
    server_name avocado.example.com;  # この行を追加する
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
# => Which names for HTTPS: avocado.example.comが候補に登場するのでそれを選ぶ

# 自動的にSSL証明書が更新されるか、ドライランを行う
sudo certbot renew --dry-run
```

Certbot は Nginx の設定ファイル (`/etc/nginx/sites-available/default`) を更新する

ブラウザで `https://avocado.example.com` にアクセスすると、Nginx のデフォルトページが表示される

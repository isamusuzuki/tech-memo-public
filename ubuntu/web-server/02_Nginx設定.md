# Nginx 設定

作成日 2021/11/03、更新日 2023/04/10

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

後の Certbot (Let's Encrypt のツール) がスムーズに進行するように、最低限の編集を行う

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

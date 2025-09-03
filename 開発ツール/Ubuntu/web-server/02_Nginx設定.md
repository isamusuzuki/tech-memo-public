# Nginx 設定

作成日 2021/11/03、更新日 2023/04/12

## 01. 目的

下記の構成で、インターネット上に Web サーバーを構築する

- OS ... Ubuntu 22.04 LTS
- Webサーバー ... Nginx

### 前提条件

DNSサーバーには `avocado.example.com` というホスト名がすでに登録されているものとする

## 02. Nginx のインストール

```bash
sudo apt install -y nginx

nginx -v
# => nginx version: nginx/1.18.0 (Ubuntu)
```

## 03. Nginx 設定ファイルの編集

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

## 04. Nginxの動作確認

```bash
# シンタックスエラーをチェックする
sudo nginx -t

# Nginxを再起動する
sudo systemctl restart nginx
```

ブラウザで `http://avocado.example.com` にアクセスすると、Nginx のデフォルトページが表示される

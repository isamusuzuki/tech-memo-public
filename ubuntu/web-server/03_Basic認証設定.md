# Basic 認証 を設定する

作成日 2021/01/30

## 01. 目的

3ヶ所ある自社オフィス以外からのアクセスには、Basic 認証を要求する

### 前提条件

自社オフィスのIPアドレスは、`100.100.100.101`,`100.100.100.102`, `100.100.100.103` とする

## 02. ユーザー・パスワードを設定する

```bash
# htpasswdコマンドのインストール
sudo apt install apache2-utils

#.htpasswdファイルの作成
sudo htpasswd -c /etc/nginx/.htpasswd USERNAME
# => New password: PASSWORD
```

## 03. Nginx 設定ファイルを編集する

/etc/nginx/sites-available/avocado

```bash
server {
  listen 80 default_server;
  listen [::]:80 default_server;
  server_name avocado.example.com;
  return 301 https://$host$request_uri;
}

# geoブロックで、条件設定を行う
geo $authentication {
  default "Authentication required";
  100.100.100.101 "off";
  100.100.100.102 "off";
  100.100.100.103 "off";
}

server {
  listen [::]:443 ssl ipv6only=on;
  listen 443 ssl;
  server_name avocado.example.com;
  ssl_certificate /etc/letsencrypt/live/avocado.example.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/avocado.example.com/privkey.pem;
  include /etc/letsencrypt/options-ssl-nginx.conf;
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

  location / {
    auth_basic $authentication;                 # この行を追加する
    auth_basic_user_file /etc/nginx/.htpasswd;  # この行を追加する
    include proxy_params;
    proxy_pass http://unix:/home/ubuntu/avocado/avocado.sock;
  }
}
```

Nginxを再起動する

```bash
sudo systemctl restart nginx
```

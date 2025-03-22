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
# geoブロックで、条件設定を行う
geo $authentication {
  default "Authentication required";
  100.100.100.101 "off";
  100.100.100.102 "off";
  100.100.100.103 "off";
}

server {
  # ...途中省略...

  location / {
    auth_basic $authentication;                 # この行を追加する
    auth_basic_user_file /etc/nginx/.htpasswd;  # この行を追加する
    include proxy_params;
    proxy_pass http://unix:/home/ubuntu/avocado/avocado.sock;
  }
}

# ...後半省略...
```

Nginxを再起動する

```bash
sudo systemctl restart nginx
```

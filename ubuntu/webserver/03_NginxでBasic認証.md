# Nginx で Basic 認証をかける

作成日 2020/09/01

## 参考記事を写経する

[Nginx で Basic 認証 \- Qiita](https://qiita.com/kotarella1110/items/be76b17cdbe61ff7b5ca)

[リバースプロキシサーバとして使ってる Nginx で Basic 認証をかける\(サブドメイン対応\) \- Qiita](https://qiita.com/yanchi4425/items/537196ac52ff72bf1275)

```bash
# htpasswdコマンドのインストール
sudo apt apache2-utils

#.htpasswdファイルの作成
sudo htpasswd -c /etc/nginx/.htpasswd USERNAME
# => New password: PASSWORD
```

Nginx に組み込む

/etc/nginx/sites-available/FILE-NAME

```text
server {
    location / {
        auth_basic "Restricted";
        auth_basic_user_file /etc/nginx/.htpasswd;
        include proxy_params;
        proxy_pass http://unix:/home/USER-NAME/PROJECT-NAME/xxxx.sock;
    }
}
```

アプリケーションサーバーに転送する設定と同居できるのか心配だったが、杞憂であった

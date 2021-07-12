# CORS 問題解決

作成日 2021/05/21、更新日 2021/07/12

## どんな問題が発生したのか

Basic 認証を導入している Web サーバーで、CORS を追加設定しようとしたら、なにをどうやっても「Access-Control-Request-Method ヘッダーが見つからない」というエラーになって、先に進めない。ブラウザではない API テストツールを使うと、ちゃんと返事が返ってくるし、CORS 用のヘッダーも追加されているので、Python Flask での設定ミスではない

## 問題解決にとても役立った参考記事 2 本

- [Basic 認証の領域へプリフライトでリクエストしてハマった \- Qiita](https://qiita.com/yana1316/items/34ee351adb6091ea7fc1)
- [API サーバを立てるための CORS 設定決定版 \- Qiita](https://qiita.com/hirohero/items/886733f50f37404235db)

## なにが問題だったのか

- CORS に該当するリクエストの場合は、ブラウザが勝手にプリフライトリクエストを投げる
- プリフライトリクエストは、OPTIONS メソッドである
- その時、カスタムヘッダーは削除される
- カスタムヘッダーがなかったら、Basic 認証 は突破できない

## 解決方法

### プリフライトリクエストは、Nginx で処理する

/etc/nginx/sites-available/avocado

```bash
server {
  location / {
    if ($request_method = 'OPTIONS') {
        add_header Access-Control-Allow-Origin '*';
        add_header Access-Control-Allow-Methods 'GET, POST, PUT, DELETE';
        add_header Access-Control-Allow-Headers 'Origin, Authorization, Accept, Content-Type';
        add_header Access-Control-Max-Age 3600;
        add_header Content-Type 'text/plain charset=UTF-8';
        add_header Content-Length 0;
        return 204;
    }

    auth_basic $authentication;
    auth_basic_user_file /etc/nginx/.htpasswd;
    include proxy_params;
    proxy_pass http://unix:/home/ubuntu/avocado/avocado.sock;
  }
}
```

- 【注意】 値のカンマ `,` 後の空白は必須

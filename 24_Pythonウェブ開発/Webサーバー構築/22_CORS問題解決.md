# CORS 問題とその解決方法

作成日 2021/05/21、更新日 2023/04/21

## 01. どんな問題が発生したのか

Basic 認証を導入している Web サーバーで、CORS を追加設定しようとしたら、なにをどうやっても「Access-Control-Request-Method ヘッダーが見つからない」というエラーになって、先に進めない。ブラウザではない API テストツールを使うと、ちゃんと返事が返ってくるし、CORS 用のヘッダーも追加されているので、Python Flask での設定ミスではない

## 02. 問題解決にとても役立った参考記事

- [Basic 認証の領域へプリフライトでリクエストしてハマった \- Qiita](https://qiita.com/yana1316/items/34ee351adb6091ea7fc1)
- [API サーバを立てるための CORS 設定決定版 \- Qiita](https://qiita.com/hirohero/items/886733f50f37404235db)

## 03. なにが問題だったのか

- CORS に該当するリクエストの場合は、ブラウザが勝手にプリフライトリクエストを投げる
- プリフライトリクエストは、OPTIONS メソッドである
- その時、カスタムヘッダーは削除される
- カスタムヘッダーがなかったら、Basic 認証 は突破できない

## 04. 解決方法

### 04-1. プリフライトリクエストは、Nginx で処理する

/etc/nginx/sites-available/avocado

```bash
server {
  location / {
    if ($request_method = 'OPTIONS') {
        add_header Access-Control-Allow-Origin '*';
        add_header Access-Control-Allow-Methods 'GET, POST, PUT, DELETE, OPTIONS';
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

### 04-2. 注意点

- add_header 行の右端にある、値の中の、カンマ `,` 後の空白は必須。空白を詰めたらエラーが起こる

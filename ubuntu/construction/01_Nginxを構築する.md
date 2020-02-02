# Nginx サーバーを構築する

作成日 2020/02/02

## 01. Nginx をインストールする

```bash
sudo apt install nginx
```

## 02. nginx コマンドを使う

```bash
# バージョンを調べる
nginx -v
#=> nginx version: nginx/1.12.1

# 設定ファイルの文法チェックを行う
# ついでに設定ファイルのありかがわかる
sudo nginx -t
#=> /etc/nginx/nginx.conf syntax is ok
#=> /etc/nginx/nginx.conf test is successful
```

## 03. 設定ファイルを学ぶ

### 設定ファイルの構造

-   コメント行 ... `#`で始まる行
-   ディレクティブ ... `ディレクティブ名 設定値;`となっている行
-   ディレクティブブロック ... `モジュール名 { }`となっている箇所
    -   モジュールに依存する設定は、ディレクティブブロックを使って設定する
    -   モジュールがインストールされていなければ、ディレクティブブロックは無視される

### ディレクティブの設定例

以下は、http,server,location のすべてのブロックで指定可能

ファイルアップロード時のファイルサイズの制限を 4M に設定する

```text
client_max_body_size 4M;
```

Ajax のアクセスを同じドメインに制限する

```text
add_header X-Frame-Options SAMEORIGIN;
```

サーバー情報を隠蔽する HTTP レスポンスのヘッダに Nginx のバージョンを表示しない

```text
fastcgi_param SERVER_SOFTWARE nginx;
server_tokens off;
```

キャッシュを無効にする

```text
add_header Pragma no-cache;
add_header Cache-Control no-cache;
expires -1;
```

### リダイレクトさせる

```text
location / {
  rewrite ^(.*)$ http://www.example.com$1 redirect;
}
```

最後のフラグが permanent ならば永久的な 301 リダイレクトで、\
redirect ならば、一時的な 302 リダイレクトになる

例では server ディレクティブに書き込んでいたが、実際やってみたら\
それは効かなかった。おそらくサーバー名が localhost でマッチしていないからだと思う

location ディレクティブの`/`は、「/で始まる全ての URI に一致する」という意味\
であるが、正規表現や文字列の長いものが優先的に処理されるので\
「他のどこにも一致しなかった」という意味でもある

## 04. 常時 SSL 化を行う

### HSTS (HTTP Strict Transport Security)

HTTP で接続した際に、強制的に HTTPS へリダイレクトし、以降のそのドメインへの接続はすべて HTTPS とする機能

以下を、HTTP ヘッダに含むことで実現される

```text
Strict-Transport-Security:max-age=31536000;includeSubDomains
```

31536000 = 1 年間の秒数

### ディレクティブの設定例

```text
server {
   listen 80 default_server;
   server_name   mattermost.eg-wms.com ;
   return 301 https://$server_name$request_uri;
}

server {
  listen 443 ssl http2;
  server_name    mattermost.eg-wms.com ;

  ssl on;
  ssl_certificate /etc/letsencrypt/live/mattermost.eg-wms.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/mattermost.eg-wms.com/privkey.pem;
  ssl_session_timeout 1d;
  ssl_protocols TLSv1.2;
  ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
  ssl_prefer_server_ciphers on;
  ssl_session_cache shared:SSL:50m;
}
```

## 02. Preloaded HSTS

ブラウザ本体に HSTS で接続するドメインのリストを持つ

Chrome の Preloaded HSTS に組み入れるためは、以下のサイトで登録する

[HSTS Preload List Submission](https://hstspreload.org/)

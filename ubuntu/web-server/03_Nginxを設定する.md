# Nginx を設定する

作成日 2020/02/02

## 01. 【復習】Nginx をインストールする

```bash
sudo apt install nginx
```

## 02. 【復習】nginx コマンドを使う

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

## 03. Nginx の設定ファイル

設定ファイルの構造

- コメント行 ... `#`で始まる行
- ディレクティブ ... `ディレクティブ名 設定値;`となっている行
- ディレクティブブロック ... `モジュール名 { }`となっている箇所
  - モジュールに依存する設定は、ディレクティブブロックを使って設定する
  - モジュールがインストールされていなければ、ディレクティブブロックは無視される

### ディレクティブの設定例

以下は、http,server,location のすべてのブロックで指定可能

#### ファイルアップロード時のファイルサイズの制限を 4M に設定する

```text
client_max_body_size 4M;
```

#### Ajax のアクセスを同じドメインに制限する

```text
add_header X-Frame-Options SAMEORIGIN;
```

#### サーバー情報を隠蔽する HTTP レスポンスのヘッダに Nginx のバージョンを表示しない

```text
fastcgi_param SERVER_SOFTWARE nginx;
server_tokens off;
```

#### キャッシュを無効にする

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

最後のフラグが permanent ならば永久的な 301 リダイレクトで、redirect ならば、一時的な 302 リダイレクトになる

例では server ディレクティブに書き込んでいたが、実際やってみたら効かなかった。おそらくサーバー名が localhost でマッチしていないからだと思う

location ディレクティブの`/`は、「/で始まる全ての URI に一致する」という意味であるが、正規表現や文字列の長いものが優先的に処理されるので「他のどこにも一致しなかった」という意味でもある

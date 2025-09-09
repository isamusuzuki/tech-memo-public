# Nginx ログを見る

作成日 2021/07/12

## 01. ログのありか

```bash
cd /var/log/nginx
ls
# => access.log, access.log.1~14
# => error.log, error.log.1~14

# 古いものはgzipでアーカイブされているので、見る前に解凍する必要がある
sudo gzip -d error.log.2.gz
sudo gzip -d access.log.2.gz
```

## 02. ログフォーマットを確認する

/etc/nginx/nginx.conf

```text
##
# Logging Settings
##

access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;
```

このように `log_format` について指定がない場合は、`combined` が指定されたことになる

```text
log_format combined '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent"';
```

### 実例を見てみる

```text
100.100.100.100 - - [10/Jul/2021:10:53:39 +0900] "GET / HTTP/1.1" 401 590 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
100.100.100.100 - basic_user [10/Jul/2021:10:53:41 +0900] "GET / HTTP/1.1" 200 1289 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
```

- 一番最初が、アクセスしてきたユーザーの IP アドレス
- 次のハイフンは、飾り
- 次は、ベーシック認証のユーザー名
- 次の括弧で囲まれたところが、日付・時刻
- 次のダブルクォーテーションで囲まれたところが、リクエスト内容
- 次の数字が、HTTP ステータスコード
- 次の数字が、返信バイト数
- 次のダブルクォーテーションで囲まれたところが、リファラー
- 最後の括弧で囲まれたところが、ユーザーエージェント

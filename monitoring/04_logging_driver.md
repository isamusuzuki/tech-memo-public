# logging driver

作成日 2018/01/13

## Nginx のログはどうやって取得すればよいか

Fluentd には tails プラグインというのがあって、どんどん書き足されていくログファイルのお尻だけを読み続けていくことがわかった。しかし長期間運用したらログファイルが肥大化する恐れがある

ログファイルのローテーションは、ホストのほうで logrotate コマンドを使って処理する

[logrotate 入門 \- Qiita](https://qiita.com/zom/items/c72c7bac63462225971b)

> logrotate は複数のログファイルを圧縮、削除、メールで送信するための機能です。logrotate 自体はデーモンではなく crond によって実現されています

[Docker コンテナのログをローテートする \- Qiita](https://qiita.com/Gin/items/646b60cae9b6a2c9812a)

## Nginx のログを標準出力に出せば、docker のログ出力に任せることができるのは

[docker の log 周りの対応 \- Carpe Diem](http://christina04.hatenablog.com/entry/2016/10/27/001717)

- コンテナのログは何処に渡すべきか
- log driver を使って転送
- nginx のように、/dev/stdout, /dev/stderr へログファイルをリンクすれば良い

```bash
# forward request and error logs to docker log collector
ln -sf /dev/stdout /var/log/nginx/access.log
ln -sf /dev/stderr /var/log/nginx/error.log
```

[library/nginx \- Docker Hub](https://hub.docker.com/_/nginx/)

オフィシャルの nginx コンテナには、すでにその設定が入っていた => あとは logging driver の勉強をすればよい

## logging driver を勉強する

[Configure logging drivers \| Docker Documentation](https://docs.docker.com/engine/admin/logging/overview/)

[Fluentd logging driver \| Docker Documentation](https://docs.docker.com/engine/admin/logging/fluentd/)

[docker fluentd logging driver の基礎的な設定 \- Qiita](https://qiita.com/moaikids/items/8a8ee90e163f14e6e923)

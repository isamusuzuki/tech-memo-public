# logrotate を学ぶ

作成日 2019/12/12

## 01. logrotate とは

[logrotate 入門 \- Qiita](https://qiita.com/zom/items/c72c7bac63462225971b)

> logrotate は複数のログファイルを圧縮、削除、メールで送信するための機能です。\
> logrotate 自体はデーモンではなく crond によって実現されています

### logrotate の動作確認

```bash
# crondの動作確認
systemctl status cron

# logrotateのインストール確認
apt search logrotate
# => logrotate/bionic,now 3.11.0-0.1ubuntu1 amd64 [installed,automatic]
# =>  Log rotation utility

# logrotateの設定確認
sudo less /etc/cron.daily/logrotate
less /etc/logrotate.conf
less /etc/logrotate.d/mysql-server
```

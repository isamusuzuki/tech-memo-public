# logrotate を使う

作成日 2021/06/16

## logrotate の設定ファイル

- logrotate.d 下のファイルは、所属を `root:root` にする
- logrotate.d 下のファイルは、ファイル属性を `644` にする

```text
--/etc/
    |--logrotate.conf
    `--logrotate.d
        `--avocado
```

/etc/logrotate.conf

```text
weekly
su root adm
rotate 4
create
include /etc/logrotate.d
```

## 設定作業

/etc/logrotate.d/avocado

```text
/home/gcp-user/avocado/temp/*.log {
    ifempty
    missingok
    daily
    rotate 7
    dateext
    dateformat .%Y%m%d
}
```

## 確認作業

```bash
# dry-run をして結果をみてみる
sudo logrotate -dv /etc/logrotate.conf

# 強制実行してみる
sudo logrotate -f /etc/logrotate.conf

# 最後にローテーションされたログファイルの日付を確認する
cat /var/lib/logrotate/status
```

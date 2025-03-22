# PostgreSQL を設定する

作成日 2021/06/20

## 外部からアクセスできるようにする

`/var/lib/postgresql/data/pg_hba.conf` 設定ファイルに下の 1 行を追加する

```text
host all all 192.168.1.0/24 trust
```

`/var/lib/postgresql/data/postgresql.conf` 設定ファイルを編集する

```text
listen_addresses='localhost'
=> listen_addresses='*'
or
=> listen_addresses='192.168.1.16 192.168.1.18'
```

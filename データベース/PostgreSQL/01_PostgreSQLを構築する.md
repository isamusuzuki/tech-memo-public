# Ubuntu 20.04 LTS に PostgreSQL をインストールする

作成日 2021/06/18

## 参考記事を写経する

[Ubuntu 20\.04 への PostgreSQL のインストールおよび使用方法 \| DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-ja)

### ステップ 1 — PostgreSQL のインストール

```bash
# OSのバージョンを確認する
cat /etc/os-release
# => NAME="Ubuntu"
# => VERSION="20.04.2 LTS (Focal  Fossa)

# インストール済みパッケージを更新する
sudo apt update

# PostgreSQLをインストールする
sudo apt install postgresql postgresql-contrib

# サービス稼働の確認
systemctl status postgresql

# バージョンの確認
psql --version
# => psql (PostgreSQL) 12.7 (Ubuntu 12.7-0Ubuntu0.20.04.1)
```

### ステップ 2 — PostgreSQL のロールとデータベースの使用

```bash
# ログインしてみる
psql
# => psql: error: FATAL: role "gontaro" does not exist
```

- PostgreSQL はユーザーとグループ間の区別はせず、代わりにより柔軟な「ロール」を使う
- `Postgres`ロールに関連付けられた`postgres`というユーザーアカウントが誕生している

```bash
# 一時的にpostgresユーザーに切り替える
sudo -i -u postgres

# ログインする
psql
# => psql (PostgreSQL) 12.7 (Ubuntu 12.7-0Ubuntu0.20.04.1)
# => Type "help" for help.
# =>
# => postgres=#

# ユーザー切り替えを終了する
exit
```

- `\q`で、PostgreSQL プロンプトを終了できる
- 一連の流れを 1 回のコマンドで実行できる

```bash
# ログインする
sudo -u postgres psql
# => psql (PostgreSQL) 12.7 (Ubuntu 12.7-0Ubuntu0.20.04.1)
# => Type "help" for help.
# =>
# => postgres=#
```

### ステップ 3 — 新しいロールの作成

postgres ユーザーの時だけ使えるコマンドがある

```bash
sudo -i -u postgres

createuser --interactive
# => Enter name of role to add: focaldb
# => Shall the new role be a superuser? (y/n): y
```

### ステップ 4 — 新しいデータベースの作成

ロールはデフォルトで同じ名前のデータベースへの接続を試行する

```bash
sudo -i -u postgres

createdb focaldb
```

### ステップ 5 — 新しいロールで Postgres プロンプトを開く

- Postgres ロール、データベースと同じ名前の Linux ユーザーが必要
- postgres ユーザーは sudo 権限を持たない
- sudo 権限を持つユーザーで以下を実行する

```bash
sudo adduser focaldb
# => New password: focalpasswd
# => Retype new password: focalpasswd
```

新しいアカウントで新しいデータベースにログインしてみる

```bash
sudo -i -u focaldb
psql

focaldb=# \conninfo
# => You are connected to database "focaldb" as user "focaldb" via socket in "/var/run/postgresql" at port "5432".
```

### ステップ 6 — テーブルの作成と削除

TBF

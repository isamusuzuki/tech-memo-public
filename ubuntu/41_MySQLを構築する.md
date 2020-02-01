# MySQL サーバーを構築する

作成日 2019/7/23、更新日 2019/12/05

## 01. MySQL をインストールする

```bash
# OSのバージョンを確認する
cat /etc/os-release
# => NAME="Ubuntu"
# => VERSION="18.04.3 LTS (Bionic Beaver)"

# MySQL 5.7をインストールする
sudo apt install mysql-server-5.7

# MySQLの稼働を確認する
systemctl status mysql

# MySQLのバージョンを確認する
mysql --version
# => mysql  Ver 14.14 Distrib 5.7.28, for Linux (x86_64) using  EditLine wrapper

# rootのパスワードを設定する
sudo mysql_secure_installation

# rootでログインする
sudo mysql
```

以下は、MySQL プロンプト下で実行する

```sql
create database db default character set utf8;

create user 'dbuser'@'%' identified by 'dbpasswd@1X';

grant all privileges on db.* to 'dbuser'@'%';

flush privileges;

use mysql;

select user, Host from user;

show grants for 'dbuser'@'%';

\q
```

ターミナルに戻る

```bash
# DBへの接続を確認
mysql -u dbuser -p db
```

## 02. 外部から MySQL に接続できるようにする

```bash
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
# => [mysqld]
# => bind-address = 127.0.0.1 をコメントアウトする

sudo systemctl restart mysql
sudo systemctl enable mysql
```

## 03. 日本語を使えるようにする

現状を確認する

```sql
show variables like 'character%';
-- character_set_client: utf8
-- character_set_connection: utf8
-- character_set_database: latin1（※1）
-- character_set_filesystem: binary
-- character_set_results: utf8
-- character_set_server: latin1（※2）
-- character_set_system: utf8
-- character_sets_dir: /usr/share/mysql/charsets/
```

※1 は、現在のデータベースの文字コードの問題

```sql
use mysql;
alter database database1 default character set utf8;
use db;
```

※2 は、my.cnf の設定の問題

```bash
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
# => [mysqld]
# => character_set_server=utf8

sudo systemctl restart mysql
```

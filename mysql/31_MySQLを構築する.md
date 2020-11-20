# MySQL サーバーを構築する

作成日 2019/7/23、更新日 2020/11/20

## 01. MySQL をインストールする

```bash
# OSのバージョンを確認する
cat /etc/os-release
# => NAME="Ubuntu"
# => VERSION="20.04.1 LTS (Focal Fossa)"

# MySQL 8.0をインストールする
sudo apt install mysql-server

# MySQLの稼働を確認する
systemctl status mysql

# MySQLのバージョンを確認する
mysql --version
# => mysql  Ver 8.0.22-0ubuntu0.20.04.2 for Linux on x86_64 ((Ubuntu))

# rootのパスワードを設定する
sudo mysql_secure_installation

# rootでログインする
mysql -u root -p
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
```

## 03. 日本語を使えるようにする

現状を確認する

```sql
show variables like 'character%';
```

MySQL 8.0 では最初からユニコードの設定になっていた

```text
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8mb4                    |
| character_set_connection | utf8mb4                    |
| character_set_database   | utf8mb4                    |
| character_set_filesystem | binary                     |
| character_set_results    | utf8mb4                    |
| character_set_server     | utf8mb4                    |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
```

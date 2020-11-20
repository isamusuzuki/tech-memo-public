# MySQLを管理する

作成日 2019/11/08、更新日 2019/12/20

## 01. ユーザー管理

### 登録されているユーザーとその権限を確認する

```sql
USE mysql;

SELECT user, host FROM user;

SHOW GRANTS for dbuser@localhost;
```

### ユーザーの作成と権限付与を同時に行う

```sql
CREATE DATABASE metabase DEFAULT CHARACTER SET utf8;

GRANT ALL PRIVILEGES ON metabase.* TO 'metauser'@'localhost' IDENTIFIED BY 'metaPasswd1!';

FLUSH PRIVILEGES;
```

`'localhost'`を`'%'`にするとワイルドカードになるが、その中にlocalhostは含まれない

### ユーザーのパスワードを変更する

```sql
SET PASSWORD FOR metauser@localhost = 'metaPasswd1!';
```

## 02. スロークエリーログを出力する

現状のログ設定について調べる

```bash
mysql -u root -p
mysql> show variables like '%log%';
#=> general_log | OFF
#=> general_log_file | /var/lib/mysql/ip-172-31-34-235.log
#=> log_error | /var/log/mysqld.log
#=> slow_query_log | OFF
#=> slow_query_log_file | /var/lib/mysql/ip-172-31-34-235-slow.log

mysql> show variables like '%long%';
#=> long_query_time | 10.000000
```

/etc/my.cnf を編集する

```text
[mysqld]
slow_query_log=1
slow_query_log_file=/var/lib/mysql/slow.log
long_query_time=0.020000
```

## 03. データベースのバックアップとレストア

### データベース全体をバックアップする

`dump.sh`というスクリプトを作成し、サーバー上で実行する

```bash
host=db.abcdefg.ap-northeast-1.rds.amazonaws.com
db=db1
user=dbuser
hizuke=`date +%Y%m%d`
MYSQL_PWD=dbpasswd mysqldump -h ${host} -u ${user} ${db} > ${hizuke}_${db}.sql
```

### 特定のテーブルだけをバックアップする

`dump2.sh`というスクリプトを作成し、サーバー上で実行する

```bash
host=db.abcdefg.ap-northeast-1.rds.amazonaws.com
db=db1
user=dbuser
password=dbpasswd
hizuke=`date +%Y%m%d`
MYSQL_PWD=${password} mysqldump -h ${host} -u ${user} ${db} table1 table2 > ${hizuke}_some_tables_only.sql
```

### dumpしたデータを、別のサーバーにレストアする

```bash
# データをサーバーに持っていく
scp ./dbbackup/*.sql ec2-user@server2:~

# あらかじめデータベースとユーザーが出来ていることが条件
mysql -u dbuser -p -D db2 < 20190725_db1.sql
```

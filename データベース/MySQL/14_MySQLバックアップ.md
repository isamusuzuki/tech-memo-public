# MySQL バックアップ

作成日 2019/11/08、更新日 2021/03/02

## 01. データベース全体をバックアップする

`dump.sh`というスクリプトを作成し、サーバー上で実行する

```bash
host=db.abcdefg.ap-northeast-1.rds.amazonaws.com
db=db1
user=dbuser
hizuke=`date +%Y%m%d`
MYSQL_PWD=dbpasswd mysqldump -h ${host} -u ${user} ${db} > ${hizuke}_${db}.sql
```

## 02. 特定のテーブルだけをバックアップする

`dump2.sh`というスクリプトを作成し、サーバー上で実行する

```bash
host=db.abcdefg.ap-northeast-1.rds.amazonaws.com
db=db1
user=dbuser
password=dbpasswd
hizuke=`date +%Y%m%d`
MYSQL_PWD=${password} mysqldump -h ${host} -u ${user} ${db} table1 table2 > ${hizuke}_some_tables_only.sql
```

## 03. dumpしたデータを、別のサーバーにレストアする

```bash
# データをサーバーに持っていく
scp ./dbbackup/*.sql ec2-user@server2:~

# あらかじめデータベースとユーザーが出来ていることが条件
mysql -u dbuser -p -D db2 < 20190725_db1.sql
```

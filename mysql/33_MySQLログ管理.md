# MySQL ログ管理

作成日 2019/11/08、更新日 2021/03/02

## 01. スロークエリーログを出力する

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

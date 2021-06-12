# MySQL ユーザー管理

作成日 2019/11/08、更新日 2021/03/02

## 01. 登録されているユーザー名を確認する

```sql
USE mysql;

SELECT user, host FROM user;
```

## 02. 特定のユーザーの権限を確認する

```sql
SHOW GRANTS FOR root@'%';
SHOW GRANTS FOR dbuser@'localhost';
```

## 03. ユーザーを作成する

```sql
CREATE DATABASE metabase DEFAULT CHARACTER SET utf8;

CREATE USER 'metauser'@'localhost' IDENTIFIED BY 'metaPasswd1!';

CREATE USER 'readonly'@'%' IDENTIFIED BY 'readOnly1!';
```

`'localhost'`を`'%'`にするとワイルドカードになるが、その中に localhost は含まれない

GRANT 構文でパスワードも指定し、ユーザーの作成も行ったら、以下のような警告文が出た。今後は、CREATE USER 構文でユーザーを作成してから、GRANT 構文で権限を付与することを徹底する

```text
1 warning(s): 1287 Using GRANT for creating new user is deprecated and will be removed in future release. Create new user with CREATE USER statement.
```

## 04. ユーザーに権限を付与する

```sql
GRANT ALL PRIVILEGES ON metabase.* TO 'metauser'@'localhost';

GRANT SELECT ON metabase.* TO 'readonly'@'%';
```

## 05. ユーザーのパスワードを変更する

```sql
SET PASSWORD FOR 'metauser'@'localhost' = 'metaPasswd2!';
```

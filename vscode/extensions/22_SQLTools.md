# SQLTools

作成日 2019/12/19、更新日 2020/05/16

## 01. SQLTools とは

Visual Studio Code の拡張機能\
データベースに接続して SQL クエリーを投げて結果を取得できる

公式トップ => [SQLTools](https://vscode-sqltools.mteixeira.dev/)

MySQL Workbench から離脱したいので、不慣れでも使っていくことにした

## 02. データベース接続情報

```json
{
  "sqltools.connections": [
    {
      "connectionTimeout": 30,
      "database": "db",
      "dialect": "MySQL",
      "name": "dbconnection1",
      "password": "dbpasswd",
      "port": 3306,
      "server": "100.100.100.100",
      "username": "dbuser"
    }
  ]
}
```

## 03. SQL クエリーの実行方法

- `Ctrl + Shift + P`でコマンドパレットを出す
- `SQLTools Connection: Run this file`を実行する

## 04. テーブルの詳細を知る

MySQL Workbench では、Navigator 枠の中で、該当のテーブルをクリックすると\
Information 枠に、テーブルのカラム情報が登場した

さらに、テーブルを右クリック ＞ Send to SQL Editor ＞ Create Statement で\
Create Statement を見ることができた

この 2 つの機能がとても便利だと感じていたが、SQL クエリーの実行で\
同じことができることがわかった

```sql
-- Information枠と同じ情報を表示する
DESC m_items;

-- Create Statementを表示する
-- 1カラムの中に全文が入るので、コピペして広げる必要あり
SHOW CREATE TABLE m_items;
```

## 05. 毎回自動生成される session.sql を作成させない

```json
{
  "sqltools.autoOpenSessionFiles": false
}
```

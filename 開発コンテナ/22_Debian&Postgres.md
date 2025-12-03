# データベースを別コンテナで用意する

作成日 2025/02/11、更新日 2025/12/03

## 1. 概要

- PostgreSQLデータベースをコンテナで用意する
- もうひとつのコンテナからデータベースを操作する
- `perl: warning: Setting locale failed.`警告を出さないように設定する

## 2. ファイル＆フォルダ構造

```text
--banana/
    |--.devcontainer/
    |   |--devcontainer.json
    |   |--docker-compose.yml
    |   `--Dockerfile
    `--db_setup/
        |--data1.sql
        |--data2.csv
        `--schema.sql
```

devcontainer.json

```json
{
    "name": "App with Database",
    "dockerComposeFile": "./docker-compose.yml",
    "service": "app",
    "workspaceFolder": "/workspaces/${localWorkspaceFolderBasename}"
}
```

docker-compose.yml

```yaml
services:
  app:
    container_name: debiandev
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ../..:/workspaces:cached
    command: sleep infinity
    network_mode: service:db
  db:
    container_name: postgresdb
    image: postgres:latest
    restart: unless-stopped
    volumes:
      - postgres-data:/var/lib/postgres/data
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
volumes:
  postgres-data:
```

Dockerfile

```dockerfile
FROM debian:bookworm-slim
RUN apt-get update && apt-get install -y \
  postgresql-client \
  git \
  locales \
  && rm -rf /var/lib/apt/lists/*

RUN sed -i -E 's/# (ja_JP.UTF-8)/\1/' /etc/locale.gen \
  && locale-gen \
  && update-locale LANG=ja_JP.UTF-8
ENV LANG=ja_JP.UTF-8 LANGUAGE=ja_JP:ja LC_ALL=ja_JP.UTF-8
```

data1.sql

```sql
insert into myfriends (name, address) values ('Kudou', 'Kyoto');
insert into myfriends (name, address) values ('南助松', '空想の都市');
```

data2.csv

```csv
id,name,address,created_on
101,"John Smith","unknown","2025-02-10"
102,"桃太郎","昔昔あるところ","2025-02-10"
```

schema.sql

```sql
create table myfriends (
    id serial,
    name varchar(10),
    address varchar(10),
    created_on TIMESTAMP DEFAULT current_timestamp
);
```

## 3. ターミナル操作

```bash
# インストールの確認
psql --version

# 環境変数の確認
printenv | more

# DB接続
psql -h postgresdb -p 5432 -U postgres postgres

# SQLファイルの実行
\i db_setup/schema.sql
\i db_setup/data1.sql

# CSVファイルのインポート
\copy myfriends from db_setup/data2.csv with DELIMITER ',' CSV header

# テーブルの中身を表示
select * from myfriends;

# テーブル一覧の表示
\dt
\dt+

# DB接続解除
\q
```

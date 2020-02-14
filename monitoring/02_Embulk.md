# Embulk

作成日 2019/12/09

## 01. Embulk とは

オープンソースのバルクデータローダー\
データベース間のデータ転送を楽にする\
プラグインの追加で、様々なデータのインプット・アウトプットを可能にする

リポジトリ => [https://github.com/embulk/embulk](https://github.com/embulk/embulk)

ドキュメント => [https://www.embulk.org/docs/](https://www.embulk.org/docs/)

### Embulk Plugins のリスト

[List of Embulk Plugins by Category](https://plugins.embulk.org/)

- INPUT ... mysql, sql, kintone
- OUTPUT ... bigquery
- FILTER ... encrypt
- FILE PARSER ... jsonl

## 02. README の Quick Start を読む

### Linux & Mac & BSD

> Embulk is a Java application. Please make sure that Java SE Runtime Environment (JRE) is installed. Embulk v0.8 series runs on Java 7, and Embulk v0.9 series runs on Java 8. Java 9 is not supported in any version for the time being.

```bash
curl --create-dirs -o ~/.embulk/bin/embulk -L "https://dl.embulk.org/embulk-latest.jar"
chmod +x ~/.embulk/bin/embulk
echo 'export PATH="$HOME/.embulk/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Running example

> `embulk example` command generates a sample CSV file so that you can try embulk quickly:

```bash
embulk example ./try1
embulk guess ./try1/seed.yml -o config.yml
embulk preview config.yml
embulk run config.yml
```

### Using Plugins

```bash
embulk gem install embulk-output-command
embulk gem list
```

`config.yml`ファイル

```yaml
in:
  type: file
  path_prefix: "./try1/csv/sample_"
  ...
out:
  type: command
  command: "cat - > task.$INDEX.$SEQID.csv.gz"
  encoders:
    - {type: gzip}
  formatter:
    type: csv
```

## 03. Embulk の紹介記事を読む

[Embulk を活用して、MySQL のデータを BigQuery にロードする \- Qiita](https://qiita.com/gamu1012/items/7ef5d5b2dcc132d30380)

### embulk のインストール

```bash
curl --create-dirs -o ~/.embulk/bin/embulk -L "https://dl.embulk.org/embulk-latest.jar"
chmod +x ~/.embulk/bin/embulk
echo 'export PATH="$HOME/.embulk/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### plugin のインストール

```bash
embulk gem install embulk-input-mysql
embulk gem install embulk-output-bigquery
```

### 設定ファイル

```yaml
in:
  type: mysql
  host: { { env.DB_HOST } }
  user: { { env.DB_USER } }
  password: { { env.DB_PASS } }
  database: { { env.DB_DATABASE } }
  table: posts
  select: "id,title,body,created_at,updated_at"

out:
  type: bigquery
  location: { { env.BIG_QUERY_LOCATION } }
  mode: replace
  table: posts
  auth_method: json_key
  json_keyfile: { { env.GOOGLE_APPLICATION_CREDENTIALS } }
  project: { { env.BIG_QUERY_PROJECT } }
  dataset: { { env.BIG_QUERY_DATASET } }
  compression: GZIP
  auto_create_table: true
  allow_quoted_newlines: true
```

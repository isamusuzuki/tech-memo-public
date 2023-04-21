# Gunicorn 問題とその解決方法

作成日 2021/01/30、更新日 2023/04/21

## 01. Gunicorn サービスに環境変数を読ませる

### 01-1. 問題発生

Flask から 環境変数を読ませたときに、「その値はない」と言われた

Gunicorn サービスが起動する前に、`.env` ファイルを読み込ませる必要がある

### 01-2. 解決方法

systemd の設定ファイルには、`EnvironmentFile` という項目がある。これで `.env` ファイルを読み込ませることができる。すでに `Environment` という項目を使っているが、`Environment` と `EnvironmentFile` は、問題なく両立できる（検証済み）

/etc/systemd/system/avocado.service

```text
[Unit]
Description=Gunicorn instance to serve avocado
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/avocado
Environment="PATH=/home/ubuntu/avocado/venv/bin"
EnvironmentFile=/home/ubuntu/avocado/.env  // この行を追加する
ExecStart=/home/ubuntu/avocado/venv/bin/gunicorn --workers 3 --bind unix:avocado.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
```

## 02. Gunicorn サービスから外部コマンドを呼べるようにする

### 02-1. 問題発生

Flask から外部コマンドを実行させたときに、「実行ファイルが見つからない」と言われた

### 02-2. 解決方法

systemd の設定ファイルの `Environment`項目に `echo $PATH` の内容を追加すればいい。これで普段のターミナルと同じになる

```bash
echo $PATH
# => /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

/etc/systemd/system/avocado.service

```text
[Unit]
Description=Gunicorn instance to serve avocado
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/avocado
Environment="PATH=/home/ubuntu/avocado/venv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin"  // この行を編集する
ExecStart=/home/ubuntu/avocado/venv/bin/gunicorn --workers 3 --bind unix:avocado.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
```

# Gunicorn 高度な設定

作成日 2021/01/30

## 01. Gunicorn サービスに環境変数を読ませる

### 01-1. 問題発生

Flask から 環境変数を読ませたときに、「その値はない」と言われた

Gunicorn サービスが起動する前に、`.env` ファイルを読み込ませる必要がある

### 01-2. 解決方法

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
EnvironmentFile=/home/ubuntu/bobby/.env           // この行を追加する
ExecStart=/home/ubuntu/avocado/venv/bin/gunicorn --workers 3 --bind unix:avocado.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
```

ポイント: `Environment` 項目と `EnvironmentFile` 項目は両立できる

## 02. Gunicorn サービスから外部コマンドを呼べるようにする

### 02-1. 問題発生

Flask から外部コマンドを実行させたときに、「実行ファイルが見つからない」と言われた

### 02-2. 解決方法

/etc/systemd/system/avocado.service

```text
[Unit]
Description=Gunicorn instance to serve avocado
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/avocado
# Environment="PATH=/home/ubuntu/avocado/venv/bin" // この行をコメントアウトする
Environment="PATH=/home/ubuntu/bobby/venv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin" // この行を追加する
ExecStart=/home/ubuntu/avocado/venv/bin/gunicorn --workers 3 --bind unix:avocado.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
```

ポイント: `Environment`項目に `echo $PATH` の内容を追加する

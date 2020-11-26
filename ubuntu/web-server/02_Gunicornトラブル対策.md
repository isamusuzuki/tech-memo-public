# gunicorn トラブルシューティング

作成日 2020/09/10、更新日 2020/11/26

## 01. 【復習】gunicorn を systemd のサービスにする

以下のファイルを書く

/etc/systemd/system/bobby.service

```text
[Unit]
Description=Gunicorn instance to serve bobby
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/bobby
Environment="PATH=/home/ubuntu/bobby/venv/bin"
ExecStart=/home/ubuntu/bobby/venv/bin/gunicorn --workers 3 --bind unix:bobby.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
```

起動する

```bash
sudo systemctl start bobby
sudo systemctl enable bobby
sudo systemctl status bobby
```

## 02. 環境変数を読まない問題が発生

wsgi.py で `load_dotenv()` しているから大丈夫かと思っていたが、そもそも `python wsgi.py` を実行しているわけではないので、`__main__` パートは呼ばれていないと考えるべき

```python
from dotenv import load_dotenv

from server import app


if __name__ == "__main__":
    load_dotenv()
    app.run()
```

### 環境変数を読ませるには

Service ファイルに書き加える。`Environment`項目と`EnvironmentFile`項目は両立できる

```text
[Service]
Environment="PATH=/home/ubuntu/bobby/venv/bin"
EnvironmentFile=/home/ubuntu/bobby/.env <-- 追加
```

## 03. 外部コマンドが実行できない問題が発生

Flask から外部コマンドを実行させたときに、「実行ファイルが見つからない」と言われた。`Environment`項目に `echo $PATH` の内容を追加したら、解決した

```text
[Service]
Environment="PATH=/home/ubuntu/bobby/venv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin"
```

## 04. Zombie プロセスが発生した場合

Flask から subprocess を呼んだ場合、エラーが出ると、Zombie プロセスになってしまう

```python
import subprocess

cmd = f'./script.sh sample.py{check_param()}'
proc = subprocess.Popen(cmd, shell=True, executable='/bin/bash')
```

Zombie プロセスを対処する

```bash
# Zombieプロセスを見つける
ps aux | grep Z

kill {PID}
# => 本来ならば、これで削除できるはずだが、
# => 親プロセスがあるために死なない

sudo systemctl stop bobby
# => 親プロセスをいったん止めることで
# => はじめて子プロセスが止まる
```

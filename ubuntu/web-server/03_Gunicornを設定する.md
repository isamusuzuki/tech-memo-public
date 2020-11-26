# Gunicorn を設定する

作成日 2019/11/15、更新日 2020/11/26

「Ubuntu 18.04 + Nginx + Flask + Gunicorn」の組み合わせで、アプリケーションサーバーを立てる

参考記事を写経する

[How To Serve Flask Applications with Gunicorn and Nginx on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-18-04)

## Step 1 — Installing the Components from the Ubuntu Repositories

必要なモジュールをインストールする

```bash
sudo apt update
sudo apt install python3-pip python3-dev\
  build-essential libssl-dev libffi-dev\
  python3-setuptools
```

## Step 2 — Creating a Python Virtual Environment

Python 仮想環境を整える

```bash
sudo apt install python3-venv
mkdir ~/bobby
cd ~/bobby
python3 -m venv venv
source venv/bin/activate
```

## Step 3 — Setting Up a Flask Application

Python コードを書く

```bash
pip install gunicorn flask

vi ~/bobby/server.py
```

~/bobby/server.py

```python
from flask import Flask, jsonify

app = Flask(__name__)
app.config['JSON_AS_ASCII'] = False
app.config['JSON_SORT_KEYS'] = False

@app.route('/')
def index():
return jsonify({
  'status': 200,
  'message': 'オーケー、成功！'
})

if __name__ == '__main__':
  app.run(host='0.0.0.0')
```

動作チェックする

```bash
python server.py
#=> Ctrl+Cで停止する

vi ~/bobby/wsgi.py
```

~/bobby/wsgi.py

```python
from server import app

if __name__ == '__main__':
  app.run()
```

## Step 4 — Configuring Gunicorn

```bash
cd ~/bobby
gunicorn --bind 0.0.0.0:5000 wsgi:app
#=> Ctrl+Cで停止する

deactivate

sudo vi /etc/systemd/system/bobby.service
```

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

bobby フォルダに `bobby.sock` ファイルがあることを確認する

tcp ポートではなく、unix ソケットをバインドしているので、ブラウザでは見られない

## Step 5 — Configuring Nginx to Proxy Requests

Nginx をインストールする

```bash
sudo apt install nginx
nginx -v
# => nginx version: nginx/1.14.0 (Ubuntu)

sudo nginx -t
# => nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
# => nginx: configuration file /etc/nginx/nginx.conf test is successful

sudo vi /etc/nginx/sites-available/bobby
```

`/etc/nginx/sites-available/bobby`ファイル

```text
server {
  listen 80 default_server;
  server_name _;

  location / {
      include proxy_params;
      proxy_pass http://unix:/home/ubuntu/bobby/bobby.sock;
  }
}
```

Nginx を設定して再起動する

```bash
sudo rm /etc/nginx/sites-enabled/default
sudo ln -s /etc/nginx/sites-available/bobby /etc/nginx/sites-enabled/default

# シンタックスエラーをチェックする
sudo nginx -t

sudo systemctl restart nginx
```

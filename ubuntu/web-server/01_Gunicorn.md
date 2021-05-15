# Gunicorn 設定

作成日 2021/01/30

## 01. 目的

下記の構成で、Web サーバーを構築する

- OS ... Ubuntu 20.04 LTS
- 言語 ... Python 3.8
- Web フレームワーク ... Flask
- WSGI サーバー ... Gunicorn

### 前提条件

プロジェクトのフォルダ名は `avocado` とする

## 02. Python 環境を整える

```bash
python3 --version
# => Python 3.8.5

# 追加モジュールをインストールする
sudo apt install python3-pip python3-venv -y

# プロジェクトのフォルダを作成する
cd ~
mkdir avocado
cd avocado

# venv 仮想環境を作成する
python3 -m venv venv
source venv/bin/activate
pip install wheel

# FlaskとGunicornをインストールする
pip install Flask gunicorn
```

## 03. Python スクリプトを書く

~/avocado/server.py

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
```

~/avocado/wsgi.py

```python
from server import app

if __name__ == '__main__':
  app.run()
```

動作チェックする

```bash
gunicorn --bind 0.0.0.0:5000 wsgi:app
```

ブラウザで `http://localhost:5000` にアクセスすると、JSON 文字列が表示される

`Ctrl+C` で停止する

## 04. Gunicorn を systemd のサービスにする

systemd の設定ファイルを編集する

```bash
sudo vi /etc/systemd/system/avocado.service
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
Environment="PATH=/home/ubuntu/avocado/venv/bin"
ExecStart=/home/ubuntu/avocado/venv/bin/gunicorn --workers 3 --bind unix:avocado.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
```

Gunicorn サービスを起動する

```bash
sudo systemctl start avocado
sudo systemctl enable avocado
sudo systemctl status avocado
```

avocado フォルダに `avocado.sock` ファイルがあることを確認する

tcp ポートではなく、unix ソケットをバインドしているので、ブラウザでは見られない

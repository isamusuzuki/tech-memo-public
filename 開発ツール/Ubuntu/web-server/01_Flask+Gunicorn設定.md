# Flask+Gunicorn 設定

作成日 2021/11/03、更新日 2023/04/12

## 01. 目的

下記の構成で、Web サーバーを構築する

- OS ... Ubuntu 22.04 LTS
- 言語 ... Python 3.10
- Web フレームワーク ... Flask
- WSGI サーバー ... Gunicorn

### 前提条件

プロジェクトのフォルダ名は `avocado` とする

## 02. Python 環境を整える

```bash
python3 --version
# => Python 3.10.6

# 追加モジュールをインストールする
sudo apt install -y python3-pip python3-venv

# プロジェクトのフォルダを作成する
cd ~
mkdir avocado
cd avocado

# venv 仮想環境を作成する
python3 -m venv venv
source venv/bin/activate

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
        'status': 200, 'message': 'オーケー、成功！'
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

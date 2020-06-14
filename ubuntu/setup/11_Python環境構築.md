# Ubuntu に Python 環境を構築する

作成日 2020/05/18

## 01. グローバル環境での作業

```bash
python --version
# => なし

Python3 --version
# => Python 3.8.2

pip3 --version
# => なし

sudo apt install -y python3-pip python3-venv
```

## 02. プロジェクト環境での作業

### .gitignore ファイルの用意

Windows と両用できるようにしている

```text
__pycache__/
bin/
[Ii]nclude/
[Ll]ib/
lib64
Scripts/
share/
pyvenv.cfg
```

### 仮想環境の構築

```bash
cd ~/PROJECTNAME

python3 -m venv .

# 仮想環境の有効化
source bin/activate
```

### パッケージのインストール

requirements.txt の例

waitress は Windows で動かすため\
gunicorn は Linux 専用なので別途インストールする

```text
autopep8
dropbox
fire
flake8
flake8-import-order
Flask
google-cloud-firestore
google-cloud-storage
PyMySQL
python-dotenv
pytz
Pillow
waitress
```

一括インストール

```bash
# 1回目の場合
pip install -r requirements.txt

# バージョンの固定
pip freeze > constraints.txt

# 2回目以降
pip install -r requirements.txt -c constraints.txt

# Linuxだけ
pip install gunicorn
```

### vscode の設定

.vscode/settings.json の例

```json
{
  "python.pythonPath": "bin/python",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.flake8Enabled": true,
  "python.linting.flake8Path": "bin/flake8",
  "python.linting.lintOnSave": true,
  "python.formatting.provider": "autopep8",
  "python.formatting.autopep8Path": "bin/autopep8",
  "editor.formatOnSave": true
}
```

## 03. Flask の設定

Windows でも動くようにする

server.py

```python
import sys

from dotenv import load_dotenv

from flask import Flask
from flask.logging import create_logger

from waitress import serve

# デバッグモード？
IS_DEBUG = False
if len(sys.argv) > 1:
    IS_DEBUG = sys.argv[1] in ['debug', 'Debug', 'DEBUG']

# Flaskアプリの起動と設定
app = Flask(__name__)
app.config['JSON_AS_ASCII'] = False
app.config['JSON_SORT_KEYS'] = False
app.debug = IS_DEBUG
lggr = create_logger(app)

# ルーティングの設定
@app.route('/')
def index():
    return render_template('index.html')


if __name__ == "__main__":
    load_dotenv()
    print(f'DEBUG = {IS_DEBUG}')
    serve(app, host='0.0.0.0', port=80)
```

wsgi.py

```python
from dotenv import load_dotenv

from server import app

if __name__ == "__main__":
    load_dotenv()
    app.run()
```

Flask アプリの起動

```bash
# Windowsの場合
python server.py debug

# Linuxの場合
python wsgi.py debug
```

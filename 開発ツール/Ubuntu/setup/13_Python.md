# Ubuntu に Python 開発環境を用意する

作成日 2020/05/18、更新日 2022/08/31

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

```text
__pycache__/
venv/

.vscode/
```

### 仮想環境の構築

```bash
cd ~/PROJECTNAME

python3 -m venv venv

# 仮想環境の有効化
source venv/bin/activate
```

### パッケージのインストール

requirements.txt の例

```text
autopep8
flake8
flake8-import-order
Flask
gunicorn
```

一括インストール

```bash
# トラブル避けのおまじない
pip install wheel

# 1回目の場合
pip install -r requirements.txt

# バージョンの固定
pip freeze > constraints.txt

# 2回目以降
pip install -r requirements.txt -c constraints.txt
```

### vscode の設定

.vscode/settings.json の例

```json
{
  "python.pythonPath": "venv/bin/python",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.flake8Enabled": true,
  "python.linting.flake8Path": "venv/bin/flake8",
  "python.linting.lintOnSave": true,
  "python.formatting.provider": "autopep8",
  "python.formatting.autopep8Path": "venv/bin/autopep8",
  "editor.formatOnSave": true,
  "terminal.integrated.env.linux": {
    "PYTHONPATH": "/home/{{YOURNAME}}/{{PROJECTNAME}}"
  }
}
```

## 03. Flask の設定

server.py

```python
import sys

from flask import Flask, render_template
from flask.logging import create_logger

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
```

wsgi.py

```python
from server import app

if __name__ == "__main__":
    app.run()
```

templates/index.html

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Sandbox</title>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bulma@0.9.0/css/bulma.min.css"
    />
  </head>
  <body>
    <section class="section">
      <div class="container">
        <h1 class="title">Hello World</h1>
        <h2 class="subtitle">My first website</h2>
      </div>
    </section>
  </body>
</html>
```

### Flask アプリの起動

```bash
python wsgi.py debug
```

## 04. 【トラブルシュート】 bdist_wheel コマンドがない

### 04-1. 問題発生

プロジェクトの中で、モジュールをインストールしたときに\
`error: invalid command 'bdist_wheel'` と赤字で表示される

### 04-2. 調べてわかったこと

グローバル環境には、wheel モジュールがある

```bash
pip3 list
# => ...
# => wheel 0.34.2
```

プロジェクトの中の仮想環境には、wheel がない

```bash
pip list
# => pip           20.0.2
# => pkg-resources 0.0.0
# => setuptools    44.0.0
```

### 04-3. 解決方法

requirements.txt を使って一括インストールする前に、\
wheel をインストールしておく

```bash
pip install wheel
```

## 05. 【トラブルシュート】 ModuleNotFoundError が発生

### 05-1. 問題発生

自分が作成したモジュールが見つからない

### 05-2. sys.path を確認する

Python のインタラクティブシェルを動かす

```bash
$ python
>>> import sys
>>> sys.path
```

ここに自分のプロジェクトのルートフォルダがないのが問題\
=> PYTHONPATH 環境変数に追加すると、自動的に sys.path に追加される

### 05-3. 解決策

.vscode/settings.json に以下を追加すると、ターミナルを起動したときに\
自動的に PYTHONPATH 環境変数が追加される

```json
{
  "terminal.integrated.env.linux": {
    "PYTHONPATH": "/home/{{YOURNAME}}/{{PROJECTNAME}}"
  }
}
```

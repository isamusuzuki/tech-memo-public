---
tags: python
---

# Flaskを使いこなす

作成日 2019/11/28

## 01. Flaskとは

公式サイト => [http://flask.pocoo.org/](http://flask.pocoo.org/)

インストール => `pip install Flask`

日本語ガイド => [https://a2c.bitbucket.io/flask/](https://a2c.bitbucket.io/flask/)

> Flask は、 Jinja2 テンプレートエンジンと Werkzeug WSGI ツールキットに 依存しています。

## 02. Flaskの最初のサンプルコード

`hello.py`

```python=
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'
```

起動方法 => `FLASK_APP=hello.py flask run


## 03. スタティックファイルを使う

hello.py

```python
from os import path

from flask import Flask, Response

app = Flask(__name__)


def root_dir():
    return path.abspath(path.dirname(__file__))


def get_file(filename):
    try:
        src = path.join(root_dir(), filename)
        return open(src).read()
    except IOError as err:
        return str(err)


@app.route('/')
def hello():
    content = get_file('index.html')
    return Response(content, mimetype='text/html')
```

### img, js, cssファイルの置き場所

静的ファイルを格納するディレクトリは、デフォルトで`static`に設定されている

`static_folder`引数を使えば、変更可能

```python=
app = Flask(__name__, static_folder='resources')
```

## 04. ログを出力する

```python=
from flask import Flask
from flask.logging import create_logger

app = Flask(__name__)
# app.debug = True

lggr = create_logger(app)


@app.route('/')
def index():
    # lggr.debug('debug')
    # lggr.info('info')
    lggr.warn('warn')
    lggr.error('error')
    lggr.critical('critical')
    return 'Hello, World!'
```

`app.debug`は、デフォルトでは`False`に設定されている\
`debug()`と`info()`を使いたいときは、`app.debug = True`にする


## 05. GETパラメーターを取得する

`request.args.get()`を使う

```python=
from flask import Flask, request

app = Flask(__name__)


@app.route('/post', methods=['GET', 'POST'])
def api_post():
    namae = request.args.get('namae', default='名無し', type=str)
    return f'ハロー、{namae}さん！'
```

## 06. POSTフォームのデータを取得する

`request.form`を使う

```python=
from flask import Flask, request

app = Flask(__name__)


@app.route('/post', methods=['POST'])
def api_post():
    return f'ハロー、{request.form["namae"]}さん！'
```

### request.formとrequest.jsonの違い

request.formは、常に`werkzeug.datastructures.ImmutableMultiDict`である一方で、\
request.jsonは、状況により異なる

- データがJSON形式で送信された場合 ... `dict`
- データがJSON形式で送信されなかった場合 ... `NoneType`

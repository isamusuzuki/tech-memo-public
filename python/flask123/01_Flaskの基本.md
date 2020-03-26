# Flask の基本

作成日 2020/03/26

## 01. Flask とは

WSGI Web アプリケーション フレームワーク

公式トップ => [Flask \| The Pallets Projects](https://palletsprojects.com/p/flask/)

ドキュメント => [Welcome to Flask — Flask Documentation \(1\.1\.x\)](https://flask.palletsprojects.com/en/1.1.x/)

> Flask は、 Jinja2 テンプレートエンジンと Werkzeug WSGI ツールキットに 依存している

パッケージトップ => [Flask · PyPI](https://pypi.org/project/Flask/)

インストール => `pip install Flask`

### 最初のサンプルコード

hello.py

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'
```

Web サーバーを起動する

```bash
FLASK_APP=hello.py flask run
```

## 02. Flask のデフォルト設定

### テンプレートファイルの置き場所

テンプレートファイルを格納するディレクトリは、`templates` がデフォルトに設定されている\
`template_folder` 引数を使えば、変更可能

```python
app = Flask(__name__, template_folder='resources')
```

#### 静的ファイルの置き場所

静的ファイルを格納するディレクトリは、デフォルトで`static` がデフォルトに設定されている\
`static_folder` 引数を使えば、変更可能

```python
app = Flask(__name__, static_folder='resources')
```

## 03. テンプレートファイルの使い方

- `render_template` メソッドをインポートして使う
- 変数の挿入には、Jinja テンプレートを使う

### サンプルコード

server.py

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/detail/<int:id>')
def detail(id):
    return render_template('detail.html', id=id)
```

detail.html

```html
<input id="id" type="hidden" value="{{id}}" />
```

### 二重中括弧 `{{}}` に要注意

Vue.js でテキスト展開に使っている二重中括弧は、Jinja テンプレートの基本要素でもある\
Jinja テンプレートを通過したコンテンツに、二重中括弧は残っていないので Vue.js では使えない

## 04. HTML ファイルを静的ファイルとして使う

```python
from os import path

from flask import Flask, Response


app = Flask(__name__)


def get_file(filename):
    try:
        root_dir = path.abspath(path.dirname(__file__))
        src = path.join(root_dir, filename)
        return open(src, 'r', encoding='UTF-8').read()
    except IOError as err:
        return f'<h1>ERROR: not file found</h1><div>{err}</div>'


@app.route('/')
def index():
    content = get_file('staic/index.html')
    return Response(content, mimetype='text/html')
```

## 05. ログを出力する

```python
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

- `app.debug`は、デフォルトでは`False`に設定されている
- `debug()`と`info()`を使いたいときは、`app.debug = True`にする

## 06. 返す HTTP ステータスを指定する

タプルを戻すようにして、2 番目の要素に数字を入れる

```python
@app.route('/hello')
def hello():
    return jsonify({'message': 'hello internal'}), 500
```

Response クラスの引数で指定することも可能

```python
@app.route('/hello')
def hello():
    return Response(response=json.dumps({'message': 'hello response'}), status=500)
```

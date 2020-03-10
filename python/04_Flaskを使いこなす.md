# Flask を使いこなす

作成日 2019/11/28、更新日 2020/03/10

## 01. Flask とは

公式サイト => [http://flask.pocoo.org/](http://flask.pocoo.org/)

インストール => `pip install Flask`

日本語ガイド => [https://a2c.bitbucket.io/flask/](https://a2c.bitbucket.io/flask/)

> Flask は、 Jinja2 テンプレートエンジンと Werkzeug WSGI ツールキットに 依存しています。

### 最初のサンプルコード

hello.py

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'
```

起動方法 => `FLASK_APP=hello.py flask run`

## 02. テンプレートファイルを使う

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

### 二重中括弧に要注意

Vue.jsでテキスト展開に使っている二重中括弧は、\
jinjaテンプレートの基本要素でもある

jinjaテンプレートを通過したhtmlファイルに、\
二重中括弧は残っていないので、Vue.jsでは使えない

### html テンプレートファイルの置き場所

テンプレートファイルを格納するディレクトリは、デフォルトで`templates`に設定されている\
`template_folder`引数を使えば、変更可能

```python
app = Flask(__name__, template_folder='resources')
```

## 03. スタティックファイルを使う

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

### img, js, css ファイルの置き場所

静的ファイルを格納するディレクトリは、デフォルトで`static`に設定されている

`static_folder`引数を使えば、変更可能

```python
app = Flask(__name__, static_folder='resources')
```

## 04. ログを出力する

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

## 05. GET パラメーターを取得する

`request.args.get()`を使う

```python
from flask import Flask, request

app = Flask(__name__)


@app.route('/post', methods=['GET', 'POST'])
def api_post():
    namae = request.args.get('namae', default='名無し', type=str)
    return f'ハロー、{namae}さん！'
```

## 06. POST フォームのデータを取得する

`request.form`を使う

```python
from flask import Flask, request

app = Flask(__name__)


@app.route('/post', methods=['POST'])
def api_post():
    return f'ハロー、{request.form["namae"]}さん！'
```

### request.form と request.json の違い

request.form は、常に`werkzeug.datastructures.ImmutableMultiDict`である一方で、\
request.json は、状況により異なる

- データが JSON 形式で送信された場合 ... request.json は`dict`
- データが JSON 形式で送信されなかった場合 ... request.json は`None`

## 07. 返す HTTP ステータスを指定する

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

## 08. Blueprintを使って、ファイルを分割する

```text
--coconut/
    |--api_modules/
    |   |--cacao.py
    |   `--choco.py
    `--server.py
```

cacao.py (choco.py)

```python
from flask import Blueprint, current_app, jsonify, request

cacao = Blueprint('cacao', __name__, url_prefix='/api/cacao')

@cacao.route('/list', methods=['POST'])
def list():
    current_app.logger.debug(f'/api/cacao/list {request.method} access')
    return jsonify({'success': True})
```

server.py

```python
from api_modules.cacao import cacao
from api_modules.choco import choco

from flask import Flask, render_template


app = Flask(__name__)
app.register_blueprint(cacao)
app.register_blueprint(choco)


@app.route('/')
def index():
    return render_template('index.html')
```

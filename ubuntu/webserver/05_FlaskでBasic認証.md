# Flask で Basic 認証を有効にする（未完成）

作成日 2020/08/31、更新日 2020/09/01

## 01. Flask-HTTPAuth を使う

[Welcome to Flask\-HTTPAuth’s documentation\! — Flask\-HTTPAuth documentation](https://flask-httpauth.readthedocs.io/en/latest/)

インストール => `pip install flask-httpauth`

```python
from flask import Flask
from flask_httpauth import HTTPBasicAuth
from werkzeug.security import generate_password_hash, check_password_hash

app = Flask(__name__)
auth = HTTPBasicAuth()

users = {
    "john": generate_password_hash("hello"),
    "susan": generate_password_hash("bye")
}

@auth.verify_password
def verify_password(username, password):
    if username in users and \
            check_password_hash(users.get(username), password):
        return username

@app.route('/')
@auth.login_required
def index():
    return "Hello, {}!".format(auth.current_user())

if __name__ == '__main__':
    app.run()
```

## 02. 参考記事を読む

[Flask で Basic 認証、Digest 認証 \- Qiita](https://qiita.com/msrks/items/7de68cde6c3ab9d5e177)

3 年前の記事なので、サンプルコードにある `get_password()` は使わないほうがいいのではないかと思った

公式サイトに `get_password()` は引退と書いてあった

> Deprecated Basic Authentication Options
> Before the verify_password described above existed there were other simpler mechanisms for implementing basic authentication. While these are deprecated they are still maintained. However, the verify_password callback should be preferred as it provides greater security and flexibility.

## 03. 本番サーバーではまだ使えない

フロントサーバーとして Nginx を使っている場合、素の設定では認証情報がアプリケーションサーバーまで送られない

プロキシー先まで転送する設定を入れ込む必要があるが、まだわかっていない。したがって Nginx のほうで、ベーシック認証を入れてスタートすることにした

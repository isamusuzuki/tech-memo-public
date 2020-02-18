# Auth0

作成日 2019/11/26

## 01. Auth0 とは

統合認証プラットフォーム IDaaS (Identity as a Service)

公式トップ => [https://auth0.com/](https://auth0.com/)

公式日本語トップ => [https://auth0.com/jp/](https://auth0.com/jp/)

[認証プラットフォーム Auth0 とは？ \- Qiita](https://qiita.com/furuth/items/68c3caa3127cbf4f6b77)

> ワシントン州ベルビューに本社がある、2013 年創業の会社\
> OAuth 1.0/2.0 や OpenID Connect といったオープンな認証・認可方式に対応している。SAML にも対応\
> 主だったプラットフォーム向けの SDK を提供しているが、Auth0 の SDK を使用することは必須ではない

## 02. Auth0 と Flask を統合するチュートリアルを写経する

[Auth0 Python SDK Quickstarts: Login](https://auth0.com/docs/quickstart/webapp/python/01-login)

> This tutorial demonstrates how to add user login to a Python web Application built with the Flask framework.

実際にテストアプリを作成するので、Auth0 アカウントが必要 ＞ 会社の GitHub アカウントでログインして作成

ログイン後、Auth0 ダッシュボード ＞ 左枠 ＞ Applications ＞ すでに Default App が作成済み

Default App の Settings ページから、Domain, Client ID, Client Secret をコピペして、`.bashrc`ファイルに登録する

```bash
export AUTH0_CLIENT_ID=abcdedf123456
export AUTH0_CLIENT_SECRET=abcdedf123456
```

Allowed Callback URLs 項目に、`http://localhost:3000/callback`を追加する

Allowed Callback URLs 項目に、`http://localhost:3000`を追加する

### クライアントサイド

server.py の抜粋

```python
from authlib.integrations.flask_client import OAuth

app = Flask(__name__)

oauth = OAuth(app)

auth0 = oauth.register(
    'auth0',
    client_id=os.environ.get('AUTH0_CLIENT_ID'),
    client_secret=os.environ.get('AUTH0_CLIENT_SECRET'),
    api_base_url='https://YOUR_DOMAIN',
    access_token_url='https://YOUR_DOMAIN/oauth/token',
    authorize_url='https://YOUR_DOMAIN/authorize',
    client_kwargs={
        'scope': 'openid profile email',
    },
)

# ログインする
@app.route('/login')
def login():
    return auth0.authorize_redirect(
        redirect_uri='http://localhost:3000/callback'
    )

# コールバックを受ける
@app.route('/callback')
def callback_handling():
    auth0.authorize_access_token()
    resp = auth0.get('userinfo')
    userinfo = resp.json()

    session['jwt_payload'] = userinfo
    session['profile'] = {
        'user_id': userinfo['sub'],
        'name': userinfo['name'],
        'picture': userinfo['picture']
    }

    return redirect('/dashboard')

# ログアウトする
@app.route('/logout')
def logout():
    session.clear()
    params = {
        'returnTo': url_for('home', _external=True),
        'client_id': os.environ.get('AUTH0_CLIENT_ID')
    }
    return redirect(auth0.api_base_url + '/v2/logout?' + urlencode(params))
```

## 03. Auth0 と SPA を統合するチュートリアルを写経する（予定）

[Auth0 Vue SDK Quickstarts: Login](https://auth0.com/docs/quickstart/spa/vuejs)

```bash
npm install -g @vue/cli
vue create my-app
cd my-app
vue add router
npm install @auth/auth0-spa-js
PORT=3000 npm run serve
```

## 04. 次なる勉強課題

Mattermost は、OpenID Connect を使う\
"AuthEndpoint", "TokenEndpoint", "UserApiEndpoint"の 3 つの URL を設定する必要がある\
Auth0 での OpenID Connect の使い方を勉強せよ

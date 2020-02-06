---
tags: sso
---

# Auth0

作成日 2019/11/26

## 01. Auth0とは

統合認証プラットフォーム IDaaS (Identity as a Service)

公式トップ => [https://auth0.com/](https://auth0.com/)

公式日本語トップ => [https://auth0.com/jp/](https://auth0.com/jp/)

[認証プラットフォーム Auth0 とは？ \- Qiita](https://qiita.com/furuth/items/68c3caa3127cbf4f6b77)

>ワシントン州ベルビューに本社がある、2013年創業の会社\
>OAuth 1.0/2.0やOpenID Connectといったオープンな認証・認可方式に対応している。SAMLにも対応\
>主だったプラットフォーム向けのSDKを提供しているが、Auth0のSDKを使用することは必須ではない

## 02. Auth0とFlaskを統合するチュートリアルを写経する

[Auth0 Python SDK Quickstarts: Login](https://auth0.com/docs/quickstart/webapp/python/01-login)

>This tutorial demonstrates how to add user login to a Python web Application built with the Flask framework. 

実際にテストアプリを作成するので、Auth0アカウントが必要 ＞ 会社のGitHubアカウントでログインして作成

ログイン後、Auth0ダッシュボード ＞ 左枠 ＞ Applications ＞ すでにDefault Appが作成済み

Default AppのSettingsページから、Domain, Client ID, Client Secretをコピペして、`.bashrc`ファイルに登録する

```bash=
export AUTH0_CLIENT_ID=abcdedf123456
export AUTH0_CLIENT_SECRET=abcdedf123456
```

Allowed Callback URLs項目に、`http://localhost:3000/callback`を追加する

Allowed Callback URLs項目に、`http://localhost:3000`を追加する

### クライアントサイド

server.pyの抜粋

```python=
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


## 03. Auth0とSPAを統合するチュートリアルを写経する（予定）

[Auth0 Vue SDK Quickstarts: Login](https://auth0.com/docs/quickstart/spa/vuejs)

```bash=
npm install -g @vue/cli

vue create my-app

cd my-app

vue add router

npm install @auth/auth0-spa-js

PORT=3000 npm run serve
```

**TODO:** 続きをやるべし

## 04. 次なる勉強課題

Mattermostは、OpenID Connectを使う\
"AuthEndpoint", "TokenEndpoint", "UserApiEndpoint"の3つのURLを設定する必要がある
Auth0でのOpenID Connectの使い方を勉強せよ



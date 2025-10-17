# GitHub OAuthトークン

作成日 2025/10/17

## 1. 解説記事を2つ読む

[【GitHub】GitHub の OAuth Tokenの取得方法 #GitHub - Qiita](https://qiita.com/KaitoMuraoka/items/94d6c227a0abb5bfcc57)

[GitHub OAuth トークンを取得する (1) 処理の流れを理解する｜まくろぐ](https://maku.blog/p/ubkt3ai/)

## 2. OAuthアプリを登録する

GitHubの設定画面 ＞ Developer Settings ＞ GitHub Apps,OAuth Apps,Personal access tokens

OAuth Apps ＞ New OAuth Appボタン

| Key                        | Value                   |
| -------------------------- | ----------------------- |
| Application name           | OLocalhost 5173         |
| Homepage URL               | `http://localhost:5173` |
| Application description    | For local Development   |
| Authorization callback URL | `http://localhost:5173` |

Client IDをメモする

Client Secretを生成してメモする（二度と表示されない）

## 3. OAuthの流れを理解する

GitHubログイン画面に遷移する

`http://github.com/login/oauth/authorize?client_id={YOUR_CLIENT_ID}&scope=read:user`

リダイレクト先のページに飛ぶ。一時コード付き

`http://localhost:5173/?code={GIVEN_CODE}}`

POSTリクエストしてアクセストークンを取得する

```text
POST https://github.com/login/oauth/access_token HTTP/1.1
Content-Type: application/json
Accept: application/json

{
 "client_id": "{YOUR_CLIENT_ID}",
 "client_secret": "{YOUR_CLIENT_SECRET}",
 "code": "{GIVEN_CODE}"
}
```

アクセストークンが返ってくる

```json
{
 "access_token": "{GIVEN_ACCESS_TOKEN}",
 "scope": "read:user",
 "token_type": "bearer"
}
```

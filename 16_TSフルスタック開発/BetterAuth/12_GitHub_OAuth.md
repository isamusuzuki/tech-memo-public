# GitHub OAuthトークン

作成日 2025/10/17、更新日 2025/10/20

## 1. 解説記事を2つ読む

[【GitHub】GitHub の OAuth Tokenの取得方法 #GitHub - Qiita](https://qiita.com/KaitoMuraoka/items/94d6c227a0abb5bfcc57)

[GitHub OAuth トークンを取得する (1) 処理の流れを理解する｜まくろぐ](https://maku.blog/p/ubkt3ai/)

## 2. OAuthアプリを登録する

GitHubの設定画面 ＞ Developer Settings ＞ GitHub Apps,OAuth Apps,Personal access tokens

OAuth Apps ＞ New OAuth appボタン

| Key                        | Value                   |
| -------------------------- | ----------------------- |
| Application name           | Localhost 5173          |
| Homepage URL               | `http://localhost:5173` |
| Application description    | For local Development   |
| Authorization callback URL | `http://localhost:5173` |
| Enable Device Flow         | Stay check off          |

Client IDをメモする

Client Secretを生成してメモする（二度と表示されない）

## 3. OAuthの流れを理解する

以下のURLをクリックして、GitHubログイン画面に遷移する

`http://github.com/login/oauth/authorize?client_id={YOUR_CLIENT_ID}&scope=read:user`

Authorize {Owner}ボタンを押す

リダイレクト先のページに飛ぶ。一時コード付き

`http://localhost:5173/?code={GIVEN_CODE}`

REST CLIENT機能拡張を使って、POSTリクエストを送信する

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

アクセストークン入りのレスポンスを得る

```json
HTTP/1.1 200 OK

{
  "access_token": "{GIVEN_ACCESS_TOKEN}",
  "token_type": "bearer",
  "scope": "read:user"
}
```

# REST Client

作成日 2025/02/19

[マーケットプレース](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)

- オーナー: Huachao Mao
- 識別子: humao.rest-client

[ソースコード](https://github.com/Huachao/vscode-restclient)

## 参照サイトを読む

[【VSCode】効率的に API 開発をする Tips](https://zenn.dev/akkie1030/articles/vscode-rest-api)

基本的な使い方

- 拡張機能をインストール、`.http`か`.rest`拡張子のファイルを作成
- ファイル内にリクエスト情報を記載する
- リクエストを実行する

加えてリクエスト内容を`###`で区切ることで複数管理することもできる

リクエストボディ部分のファイル読み込み が可能。リクエストボディ部分をファイルとして切り出せる

```bash
POST  https://reqres.in/api/users?page=2 HTTP/1.1
Content-Type: application/json

< ./request.body1.json
```

プロンプト変数を使えばリクエスト送信時に値の入力が求められ対話的にリクエストを生成できる

```bash
POST  https://reqres.in/api/login
Content-Type: application/json

{
    "email": "eve.holt@reqres.in",
    "password": "cityslicka"
}

###
# @prompt token

GET https://reqres.in/api/users?page=2
Content-Type: application/json
Authorization: Bearer {{token}}
```

リクエスト変数を使えば特定のリクエストのレスポンス結果を別のリクエストの変数値として使用することができる

```bash
# @name login
POST  https://reqres.in/api/login
Content-Type: application/json

{
    "email": "eve.holt@reqres.in",
    "password": "cityslicka"
}

###
@token = {{login.response.body.$.token}}
# @name create
POST https://reqres.in/api/users
Content-Type: application/json
Authorization: Bearer {{token}}

{
    "name": "foo",
    "job": "foo"
}
```

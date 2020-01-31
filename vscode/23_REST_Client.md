# REST Client

作成日 2019/12/09

## 01. REST Client とは

vscode 拡張機能になった API 開発ツール

公式トップ => [vscode\-restclient/README\.md at master · Huachao/vscode\-restclient](https://github.com/Huachao/vscode-restclient/blob/master/README.md)

マーケットプレイス => [REST Client \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)

## 02. REST Cient の紹介ページを読む

[REST Client は変数を使うと API の環境やパラメータ変更が楽になる！](https://dev.classmethod.jp/tool/vscode-rest-client-is-good/)

> おすすめポイント
>
> - テキストファイルで管理できる
> - 複数のリクエストを同じファイルに書ける
> - 変数が使える
> - curl コマンドを作れる
> - コードスニペットを作成できる

### GET リクエスト

```text
GET https://hoge.execute-api.ap-northeast-1.amazonaws.com/Prod/version
```

### POST リクエスト

```text
POST https://hoge.execute-api.ap-northeast-1.amazonaws.com/Prod/version
content-type: application/json

{
    "id": "1234"
}
```

- POST のリクエストボディは、1 行空けて書くこと
- `###`を区切り文字として使用できる

### 環境変数の設定

`settings.json` に書き込む

```json
{
  "rest-client.environmentVariables": {
    "$shared": {
      "version": "v1",
      "email": "hoge@xxx.com"
    },
    "dev": {
      "host": "dev.hoge.jp",
      "version": "v2"
    },
    "stg": {
      "host": "stg.hoge.jp"
    },
    "prod": {
      "host": "prod.hoge.jp"
    }
  }
}
```

コマンドパレットから、`Rest Client Switch Environment`を選択し、dev,stg,prod を選ぶと環境変数を切り替えられる

#### 変数を使った例

```text
post https://{{host}}/{{version}}/users
content-type: application/json

{
    "email": {{email}}
}
```

## 03. REST Cient の紹介ページを読む 2

[REST API のテストに Postman 使ってたけど Visual Studio Code の REST Client に乗り換えた](https://blog.okazuki.jp/)

> 1 ファイルで複数のリクエストを書いておいて、個別に実行できるので\
> 特定の API をテストで叩くためのファイルを 1 つ用意しておけば、\
> そのファイル開いて送りたいリクエストに対して Send Request するだけで出来る

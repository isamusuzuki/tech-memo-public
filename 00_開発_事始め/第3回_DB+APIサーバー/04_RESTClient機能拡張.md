# REST Client機能拡張

作成日 2026/01/14

## 1. REST Clientをインストールする

[マーケットプレース](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)

識別子: humao.rest-client

### 2. REST Clientの使い方

- 拡張子が`.http`のファイルを作成する
- ファイル内にリクエスト情報（リクエストヘッダ、またはヘッダとボディ）を記載する
- ヘッダの上に表示される`Send Request`という文字をクリックすると、リクエストが送信される
- 右側に送信結果が表示される
- リクエスト情報は`###`で区切れる。ひとつのファイルに複数のリクエスト情報を書くことができる

### 3. Rest Client用ファイルの例

```bash
GET http://127.0.0.1:8787/api/todos HTTP/1.1

###
GET http://127.0.0.1:8787/api/todos/1 HTTP/1.1

###
POST http://127.0.0.1:8787/api/todos HTTP/1.1
Content-Type: application/json

{
    "title": "シリアルを買う",
    "content": "デリシア小諸店にて"
}

###
PUT http://127.0.0.1:8787/api/todos/4
Content-Type: application/json

{
    "title": "りんごを買う",
    "content": "松井農園にて"
}

###
DELETE http://127.0.0.1:8787/api/todos/4 HTTP/1.1
```

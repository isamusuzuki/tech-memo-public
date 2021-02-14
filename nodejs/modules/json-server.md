# JSON Server

作成日 2021/02/14

JSON データを返すモックサーバー

公式サイト => [https://github.com/typicode/json-server](https://github.com/typicode/json-server)

インストール `npm i -D json-server`

## 簡単な使い方例

```text
--PROJECT
    |--db.json
    `--routes.json
```

db.json

- キーが URL となり、バリューが戻り値となる

```json
{
  "posts": [{ "id": 1, "title": "json-server", "author": "typicode" }],
  "comments": [{ "id": 1, "body": "some comment", "postId": 1 }],
  "profile": { "name": "typicode" }
}
```

routes.json

```json
{
  "/api/*": "/$1"
}
```

JSON Server を起動する

- デフォルトのポートは 3000

```bash
npx json-server --watch db.json  --routes routes.json

curl http://localhost:3000/api/posts
curl http://localhost:3000/api/posts/1
curl http://localhost:3000/api/comments
curl http://localhost:3000/api/comments/1
curl http://localhost:3000/api/profile
```

## Webpack-dev-server と json-server を組み合わせるには

Webpack の devServer 設定に proxy オプションがある

[DevServer \| webpack](https://webpack.js.org/configuration/dev-server/)

webpack.config.js

```javascript
module.exports = {
  //...
  devServer: {
    proxy: {
      '/api': 'http://localhost:3000',
    },
  },
};
```

これで、`/api/users` へのリクエストは、`http://localhost:3000/api/users` に転送される

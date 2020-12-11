# [axios](https://github.com/axios/axios)を使う

Promise ベースの HTTP クライアント

作成日 2018/11/19、更新日 2019/03/20

## DELETE 送信でデータを送るときの注意点

[$axios\.$delete でデータを送る方法 \- Qiita](https://qiita.com/yfujii1127/items/991ae9ff29b478a55b1c)

> \$delete の際は引数で「data(データ)オブジェクト」を定義する必要があるようです。

```js
// このコードはサーバーサイドでパラメーターを取得できずにエラーになる
axios.delete(
  `${context.rootState.settings.serverUrl}/portal/admin/api/user`,
  { user_id: payload.willDeleteUserId },
  { headers: { Authorization: `Bearer ${context.rootState.auth.token}` } }
);
// configの中で、dataキーの値として設定する必要あり
axios.delete(
  `${context.rootState.settings.serverUrl}/portal/admin/api/service`,
  {
    data: { service_name: payload.willDeleteService },
    headers: { Authorization: `Bearer ${context.rootState.auth.token}` }
  }
);
```

`axios.delete(url[,config])`は、`axios.get(url[,config])`と同じように、引数は 2 つしかないことがわかった

## CDN が使える

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

## GET リクエストを送信する

```js
axios
  .get("/user?ID=12345")
  .then(function(response) {
    // hanndle success
    console.log(response);
  })
  .catch(function(error) {
    // handle error
    console.log(error);
  })
  .then(function() {
    // always executed
  });
```

## POST リクエストを送信する

```js
axios
  .post("/user", {
    firstName: "Fred",
    lastName: "Flintstone"
  })
  .then(function(response) {
    // hanndle success
    console.log(response);
  })
  .catch(function(error) {
    // handle error
    console.log(error);
  });
```

### axios API

ベーシック認証などもサポートしているので、さらに高度な設定が必要なときは公式サイトを読むこと

## 参考記事

[axios の導入と簡単な使い方](https://qiita.com/ksh-fthr/items/2daaaf3a15c4c11956e9)

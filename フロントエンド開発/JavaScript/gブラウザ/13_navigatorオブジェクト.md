# navigatorオブジェクト

作成日 2021/01/26

## スマホならばspページに転送させる

- ユーザーエージェントを確認して、スマホならばSPページに転送する
- ユーザーエージェントは、`navigator.userAgent`で取得できる
- 内部パスは、`location.pathname`で取得できる
- GETメソッドの引数は、`location.search`で取得できる
- 何か引数がついているならば、転送するときにそれもつけておく

```javascript
let redirectPass = '/sp/index.html';
if (location.search.length >= 1) {
  redirectPass += location.search;
}
const agent = navigator.userAgent;
if (
  agent.search(/iPhone/) != -1 ||
  agent.search(/iPad/) != -1 ||
  agent.search(/Android/) != -1
) {
  location.href = redirectPass;
}
```

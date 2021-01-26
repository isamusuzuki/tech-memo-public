# ブラウザで使う JS

作成日 2021/01/26

## 01. window オブジェクトとは

window オブジェクトは、ブラウザに搭載されている JavaScript に、あらかじめ用意されている

window オブジェクトのプロパティにアクセスするときに `windows.` を省略できる

### window オブジェクトの例

- window.document
- window.localStorage
- window.navigator

## 02. JavaScript で DOM を操作する

### DOM 操作のキホン

```javascript
// カレントノードの最後の子として、指定ノードを追加する
parentNode.appendChild(newNode);

// カレントノードの子として、参照先ノードの前に、指定ノードを挿入する
parentNode.insertBefore(newNode, referenceNode);
```

### body タグの先端と末尾にそれぞれノードを追加する

```javascript
var h1 = document.createElement('div');
h1.setAttribute('id', 'header');
h1.innerHTML = 'header';

var f1 = document.createElement('div');
f1.setAttribute('id', 'footer');
f1.innerHTML = 'footer';

document.body.insertBefore(h1, document.body.firstChild);
document.body.appendChild(f1);
```

## 03. navigator オブジェクトを使う

### スマホならば sp ページに転送させる

- ユーザーエージェントを確認して、スマホなら SP ページに転送する
- ユーザーエージェントは、`navigator.userAgent`で取得できる
- 内部パスは、`location.pathname`で取得できる
- GET メソッドの引数は、`location.search`で取得できる
- 何か引数がついているならば、転送するときにそれもつけておく

```javascript
var redirectPass = '/sp/index.html';
if (location.search.length >= 1) {
  redirectPass += location.search;
}
var agent = navigator.userAgent;
if (
  agent.search(/iPhone/) != -1 ||
  agent.search(/iPad/) != -1 ||
  agent.search(/Android/) != -1
) {
  location.href = redirectPass;
}
```

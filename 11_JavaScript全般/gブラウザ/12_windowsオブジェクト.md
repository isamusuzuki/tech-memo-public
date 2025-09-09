# windowオブジェクト

作成日 2021/01/26、更新日 2025/08/28

## windowとは？

公式ドキュメント（英語） => [Window](https://developer.mozilla.org/en-US/docs/Web/API/Window)

- windowオブジェクトは、ブラウザに搭載されているJavaScriptにあらかじめ用意されている
- windowオブジェクトのプロパティにアクセスするときは、`windows.`を省略できる

windowオブジェクトのプロパティの例

- window.document
- window.location
- window.localStorage
- window.navigator

## navigatorプロパティ

公式ドキュメント（英語） => [Window: navigator property](https://developer.mozilla.org/en-US/docs/Web/API/Window/navigator)

### スマホならばspページに転送させる

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

## window.chromeについて

### AIによる概要

window.chromeは、JavaScriptにおいて特定のブラウザの情報を取得するために使用されていました。しかし、これは現在では推奨されない方法です。

window.chromeは、もともとChromeブラウザであるかどうかを判定するために使われていました。

### window.chromwが検索してでてこない理由

[!window.chrome && 'WebkitAppearance' in document.documentElement.style の意味がわからない](https://teratail.com/questions/333612)

> window.chromeはChromiumのバグ周辺の情報から察するとChrome拡張機能やChrome Apps用のAPIが間違って一般のウェブページに見えてしまっている状態で、Googleは消したいけども何らかの理由で簡単には消せないようです。
>
> 以前はGoogle Chromeかどうかの判定に使えたのかもしれません。なんにせよ非標準であり将来は消えるかもしれないプロパティなので、参照すべきではありません。

# `reduce()`メソッド

作成日 2025/08/06

## 1. Array.prototype.reduce()

[Array.prototype.reduce() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

> `reduce()`はArrayインターフェイスのメソッドで、配列のそれぞれの要素に対して、ユーザーが提供した「縮小」コールバック関数を呼び出します。その際、直前の要素の計算結果の返値を渡します。配列のすべての要素に対して「縮小」コールバック関数を実行した最終結果は、単一の値となります。

## 2. 配列のreduceメソッドを使う

```js
// 配列の左から右へ、2つの値に対して同時に関数を適用し、最後に単一の値にする
// 元となる配列があって、それを累積して新しい単一の値を作りたいときに使う
// 2つの値のうち、左は常に累積値となる。右に来るのが新しい値

// reduceを使わない場合
function sum(a) {
  let b = 0;
  for (var i = 0; i < a.length; i++) {
    b += a[i];
  }
  return b;
}

// reduceを使うと
function sum(a) {
  return a.reduce(function (x, y) {
    return x + y;
  });
}
let arr = [1, 2, 3, 4, 5];
sum(arr); //=> 15
```

# `sort()`メソッド

作成日 2025/04/30

## 1. Array.prototype.sort()

[Array.prototype.sort() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

> `sort()`はArrayのメソッドで、配列の要素をその場(in-place)でソートし、ソートされた同じ配列の参照を返します。
>
> `compareFn`が与えられなかった場合、`undefined`以外のすべての配列要素は文字列に変換され、文字列が UTF-16コード単位順でソートされます。`undefined`の要素はすべて、配列の末尾に並べられます。
>
> `compareFn`が与えられた場合、`undefined`以外のすべての配列要素は比較関数の返値に基づきソートされます（`undefined`の要素はすべて、`compareFn`を呼び出すことなく配列の末尾へ並べられます）。

## 2. `sort()`の考え方

```javascript
const numbers = [1,3,5,7,9];

// 昇順にする
numbers.sort((a, b) => {
  return a - b;
});

// 降順にする
stations.sort((a, b) => {
  return b - a;
});
```

- 書き方は、極めて単純。前に並べたいものを先にした引き算をリターンするだけ
- 動きは、値がプラスならばひっくり返す。値がマイナスならばそのまま
- `[1, 3]`の配列を例に考えてみる
  - `a - b`のとき、`1 - 3 = -2`になるので、そのまま`[1,3]`となる
  - `b - a`のとき、`3 - 1 = 2`になるので、ひっくり返して`[3,1]`となる

## 3. 連想配列をソートする

```javascript
const stations = [
  { name: 'Shinjuku', customers: 736715 },
  { name: 'Ikebukuro', customers: 544222 },
  { name: 'Shibuya', customers: 403277 },
  { name: 'Ueno', customers: 172306 },
];

// nameをアルファベット順にする
stations.sort(function (a, b) {
  let a1 = a['name'];
  let b1 = b['name'];
  if (a1 < b1) return -1; // -1はaをbの前へ移動
  if (a1 > b1) return 1;  // 1はaをbの後ろへ移動
  return 0;               // 0はaとbの並び替えなし
});

// customersを多い順にする
stations.sort(function (a, b) {
  let a1 = a['customers'];
  let b1 = b['customers'];
  if (a1 < b1) return 1;  // 1はaをbの後ろへ移動
  if (a1 > b1) return -1; // -1はaをbの前へ移動
  return 0;               // 0はaとbの並び替えなし
});
```

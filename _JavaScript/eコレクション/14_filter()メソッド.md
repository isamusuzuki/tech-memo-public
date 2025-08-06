# `filter()`メソッド

作成日 2025/08/06

## 1. Array.prototype.filter()

[Array.prototype.filter() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

> `filter()`はArrayインスタンスのメソッドで、指定された配列の中から指定された関数で実装されているテストに合格した要素だけを抽出したシャローコピーを作成します。
>
> `filter()`メソッドは反復処理メソッドです。指定された`callbackFn`関数を配列の各要素に対して一度ずつ呼び出し、`callbackFn`が真値を返したすべての要素からなる新しい配列を生成します。
>
> filter() メソッドはコピーメソッドです。これは this を変更するのではなく、元の配列と同じ要素を格納したシャローコピーを返します。

## 2. `filter()`は新しい配列を生成する

```javascript
const words = ["spray", "elite", "exuberant", "destruction", "present"];

const result = words.filter((word) => word.length > 6);

console.log(result);
// => Array ["exuberant", "destruction", "present"]

console.log(words);
// => Array ["spray", "elite", "exuberant", "destruction", "present"]
```

## 3. 配列をユニークな値だけに絞る

```js
// 配列の中でその値が一番最初に登場する順番と自分の順番が同じものだけに絞る
// 2回目・3回目に登場するものは一番最初に登場する順番と自分の順番とが異なる
let a = [1, 2, 3, 3, 2, 2, 5];
let b = a.filter(function (item, index, self) {
  return self.indexOf(item) === index;
}); //=> [1, 2, 3, 5]
```

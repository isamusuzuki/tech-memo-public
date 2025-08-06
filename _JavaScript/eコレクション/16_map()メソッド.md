# `map()`メソッド

作成日 2025/08/06

## 1. 配列のmapメソッドで、async/awaitを使う

- mapメソッドで呼び出す関数には、`async`をつける
- これで、その関数の中では、`await`が使える
- mapメソッド全体を、`Promise.all()`で囲む
- `Promise.all()`に`await`をつけて、結果の配列に入れる

```javascript
const result_list = await Promise.all(
  param_list.map(async (param) => {
    const result = await doSomethingPromise(param);
    return result;
  })
);
```

## 2. 二次元配列の縦横を入れ替える

```js
const transpose = (x) => {
  return Object.keys(x[0]).map((column) => {
    return x.map((row) => {
      return row[column];
    });
  });
};

const a = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
];

transpose(a);
//=> [[1,4,7],[2,5,8],[3,6,9]]

const b = [[1, 2, 3]];

transpose(b);
//=> [[1],[2],[3]]

// 省けるものは全て省いた短縮バージョン
const trnsps = (x) =>
  Object.keys(x[0]).map((column) => x.map((row) => row[column]));
```

`Object.keys()`は、連想配列のキーの値を配列にして返す。引数が配列であれば、0から始まる整数の配列が返る

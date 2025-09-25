# Array.fromAsync()

作成日 2025/09/25

[Array.fromAsync() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/fromAsync)

`Array.fromAsync()`は静的メソッドで、非同期反復可能オブジェクト、反復可能オブジェクト、配列風のオブジェクトから、シャローコピーされた新しい配列インスタンスを作成します。

構文: `Array.fromAsync(items, {mapFn}, {thisArg})`

引数:

- items ... 配列に変換する非同期反復可能、反復可能、配列風オブジェクト
- mapFn ... 省略可。配列の各要素に対して呼び出す関数
- thisArg ... 省略可。mapFn実行時にthisとして使用する値

返値: 新しいPromiseで、その履行値は新しいArrayインスタンス

```javascript
const asyncIterable = (async function* () {
  for (let i = 0; i < 5; i++) {
    await new Promise((resolve) => setTimeout(resolve, 10 * i));
    yield i;
  }
})();

Array.fromAsync(asyncIterable).then((array) => console.log(array));
// [0, 1, 2, 3, 4]
```

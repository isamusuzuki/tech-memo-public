# `localeCompare()`メソッド

[String.prototype.localeCompare()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)

> `localeCompare()` は String 値のメソッドで、参照文字列がソート順で指定された文字列の前か後か、または同じかを示す数値を返します。

返値

- `referenceStr` が `compareString` より前に出現するものである場合は負の数です。
- `referenceStr` が `compareString` より後に出現するものである場合は正の数です。等しい場合は 0 です。

```javascript
const users = [
  { name: 'John', age: 30 },
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 35 }
];

// 年齢で昇順ソート
users.sort((a, b) => a.age - b.age);

// 名前でソート
users.sort((a, b) => a.name.localeCompare(b.name));
```

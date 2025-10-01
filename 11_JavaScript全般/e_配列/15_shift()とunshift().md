# `shift()`メソッドと`unshift()`メソッド

作成日 2025/09/09

## 1. Array.prototype.shift()

[Array.prototype.shift() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)

> `shift()` は Array インスタンスのメソッドで、配列から最初の要素を取り除き、その要素を返します。このメソッドは配列の長さを変えます。

```javascript
const animals = ["pigs", "goats", "sheep"];
const first = animals.shift();

console.log(first); // "pigs"
console.log(animals); // Array ["goats", "sheep"]
```

## 2. Array.prototype.unshift()

[Array.prototype.unshift() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)

> `unshift()` は Array インスタンスのメソッドで、指定された要素を配列の先頭に追加し、新しい配列の長さを返します。

```javascript
const animals = ["pigs", "goats", "sheep"];

console.log(animals.unshift("cow")); // 4
console.log(animals); // Array ["cows", "pigs", "goats", "sheep"]
```

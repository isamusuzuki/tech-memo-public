# `slice()`メソッド

作成日 2025/09/09

## 1. Array.prototype.slice()

[Array.prototype.slice() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

> `slice()`は Array インスタンスのメソッドで、配列の一部を start から end （end は含まれない）までの範囲で、選択した新しい配列オブジェクトにシャローコピーして返します。

```javascript
const fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
const citrus = fruits.slice(1, 3);

console.log(fruits); // Array ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango']
console.log(citrus); // Array ['Orange','Lemon']
```

返値 = 取り出された要素を含む新しい配列

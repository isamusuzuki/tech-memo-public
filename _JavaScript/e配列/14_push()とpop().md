# `push()`メソッドと`pop()`メソッド

作成日 2025/09/09

## 1. Array.prototype.push()

[Array.prototype.push() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

> `push()` は Array インスタンスのメソッドで、配列の末尾に指定された要素を追加します。また返値として配列の新しい長さを返します。

```javascript
const animals = ["pigs", "goats", "sheep"];

console.log(animals.push("cows")); // 4
console.log(animals); // Array ["pigs", "goats", "sheep", "cows"]
```

## 2. Array.prototype.pop()

[Array.prototype.pop() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)

> `pop()` メソッドは、配列から最後の要素を取り除き、その要素を返します。このメソッドは配列の長さを変化させます。

```javascript
const plants = ["broccoli", "cauliflower", "cabbage", "kale", "tomato"];

console.log(plants.pop()); // "tomato"
console.log(plants); // Array ["broccoli", "cauliflower", "cabbage", "kale"]
```

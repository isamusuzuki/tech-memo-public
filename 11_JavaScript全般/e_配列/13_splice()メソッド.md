# `splice()`メソッド

作成日 2025/09/09

## 1. Array.prototype.splice()

[Array.prototype.splice() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

> `splice()` は Array インスタンスのメソッドで、その場 (in-place) で既存の要素を取り除いたり、置き換えたり、新しい要素を追加したりすることで、配列の内容を変更します。

```javascript
const months = ["Jan", "March", "April", "June"];
months.splice(1, 0, "Feb");
console.log(months) // Array ["Jan", "Feb", "March", "April", "June"]

months.splice(4, 1, "May");
console.log(months) // Array ["Jan", "Feb", "March", "April", "May"]
```

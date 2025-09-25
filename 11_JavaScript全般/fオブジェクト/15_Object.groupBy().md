# Object.groupBy()

作成日 2025/09/25

[Object.groupBy() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/groupBy)

> `Object.groupBy()`は静的メソッドで、指定されたコールバック関数によって返された文字列値に従って、指定された反復可能な要素をグループ化します。返されるオブジェクトには、グループごとに個別のプロパティがあり、グループ内の要素を含む配列が格納されます。

```javascript
const inventory = [
  { name: "asparagus", type: "vegetables", quantity: 9 },
  { name: "bananas", type: "fruit", quantity: 5 },
  { name: "goat", type: "meat", quantity: 23 },
  { name: "cherries", type: "fruit", quantity: 12 },
  { name: "fish", type: "meat", quantity: 22 },
];

const result = Object.groupBy(inventory, ({ quantity }) =>
  quantity < 6 ? "restock" : "sufficient",
);
console.log(result.restock);
// [{ name: "bananas", type: "fruit", quantity: 5 }]
```

構文: `Object.groupBy(items, callbackFn)`

引数:

- items ... 要素がグループ化される反復可能な要素
- callbackFn ... 反復可能な各要素に対して実行される関数。現在の要素のグループを示すプロパティのキー（文字列またはシンボル）に変換できる値を返す必要がある

返値: すべてのグループのプロパティを持つオブジェクトで、それぞれに関連するグループの要素を含む配列が割り当てられたもの

```javascript
const result = Object.groupBy(inventory, ({ type }) => type);

/* 結果:
{
  vegetables: [
    { name: 'asparagus', type: 'vegetables', quantity: 5 },
  ],
  fruit: [
    { name: "bananas", type: "fruit", quantity: 0 },
    { name: "cherries", type: "fruit", quantity: 5 }
  ],
  meat: [
    { name: "goat", type: "meat", quantity: 23 },
    { name: "fish", type: "meat", quantity: 22 }
  ]
}
*/
```

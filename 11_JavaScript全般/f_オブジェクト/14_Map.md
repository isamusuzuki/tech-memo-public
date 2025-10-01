# Map

作成日 2025/09/22

解説記事 => [Map<K, V>](https://typescriptbook.jp/reference/type-reuse/utility-types/record)

> `Map`はJavaScriptの組み込みAPIのひとつで、キーと値のペアを取り扱うためのオブジェクトです。`Map`にはひとつのキーについてはひとつの値のみを格納できます。

```javascript
const map = new Map<string, number>();
map.set("a", 1);
console.log(map.get("a")); // => 1
```

- TypeScriptでMapの型注釈をする場合は、`Map<string, number>`のようにMap要素の型を型変数に指定します。
- MapオブジェクトはJSON.stringifyにかけても、登録されている要素はJSONになりません。`Map`をJSON化する場合は、一度オブジェクトにする必要があります。

```javascript
const map = new Map([
  ["a", 1],
  ["b", 2],
  ["c", 3],
]);
console.log(JSON.stringify(map)); // "{}"

const obj = Object.fromEntries(map);
console.log(JSON.stringify(obj)); // "{"a":1,"b":2,"c":3}"
```

## MapとObjectの違い

解説記事 => [JavaScriptの`Map`と`Object`の違いと使い分け](https://qiita.com/oharu121/items/25cd17d1bc0b261d2f2c)

キーの種類

- Object ... キーとして使えるのは文字列またはシンボルのみ。数値やオブジェクトなどは自動的に文字列に変換される
- Map ... 任意のデータ型をキーとして使用できる

キーの順序

- Object ... キーの順序は保証されない
- Map ... 追加された順番が保持される

イテレーション

- Object ... 直接イテレートできない。`Object.keys()`や`Object.entries()`などを介する必要あり
- Map ... `for const [key, value] of map` でそのままキー・値をイテレートできる

サイズの取得

- Object .. `Object.keys(obj).length`でキー数を取得する
- Map ... `.size`プロパティでサイズを取得する

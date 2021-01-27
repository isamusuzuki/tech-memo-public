# JS オブジェクト

作成日 2021/01/27

## 01. オブジェクトの基本

空のオブジェクトを生成する

```javascript
let obj = {};
```

プロパティのあるオブジェクトを生成する

```javascript
let obj = {
  a: 1,
  b: 'abc',
  c: true,
  d: {},
  e: [],
  f: function () {},
};
```

## 02. オブジェクトの変形ルール

プロパティの名前とその値を表す変数とが同名である場合には、値の指定を省略できる

```javascript
let title = 'Programming 101';
let price = 3700;
let book = { title, price };
// {title: title, price: price}と同じ
```

オブジェクトのプロパティにメソッドを定義するときは、関数型のプロパティとして表記する必要はない

```javascript
let obj = {
  f: function (params) {}, //=> OK
  g(params) {}, //=> OK
};
```

プロパティ名を動的に生成したいときは、プロパティ名をブラケットでくくることで、式の値から動的にプロパティ名を生成できる

```javascript
let data = {
  ['hoge' + ++i]: 15,
  ['hoge' + ++i]: 20,
};
```

## 03. Object.assign()メソッド

ひとつ以上のソースオブジェクトから、直接所有で列挙可能なすべてのプロパティの値を、ターゲットオブジェクトへコピーする

戻り値はターゲットオブジェクト

```javascript
let srcObj1 = { a: 1 };
let srcObj2 = { b: 2 };
let srcObj3 = { c: 3 };
let tgtObj = {};
var newObj = Object.assign(tgtObj, srcObj1, srcObj2, srcObj3);
//=> { a:1, b:2, c:3 }
```

ターゲットオブジェクトは上書きされる。ソースオブジェクトに変化はなし

```javascript
let tgtObj = { a: 1, c: 1 };
let srcObj = { a: 2, b: 2 };
let newObj = Object.assign(tgtObj, srcObj);
//=> {a:2, c:1, b: 2}
```

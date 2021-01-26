# Lodash

作成日 2021/01/26

公式サイト => [https://lodash.com/](https://lodash.com/)

インストール `npm install --save lodash`

```javascript
const _ = require('lodash');

_.assign({ a: 1 }, { b: 2 }, { c: 3 });
//=> { 'a':1, 'b':2, 'c':3 }

_.map([1, 2, 3], function (n) {
  return n * 3;
});
//=> [3,6,9]
```

## 01. filter と find

| それぞれが返すもの   | filter   | find      |
| -------------------- | -------- | --------- |
| 見つかった場合       | 配列     | 単体      |
| 見つからなかった場合 | 空の配列 | undefined |

```javascript
// filterの例
let f = _.filter(rows, (o) => o.id === id);
if (f.length > 0) {
  // 見つかった場合（z[0]を使うこと）
}

// findの例
let f = _.find(rows, (o) => o.id === id);
if (f) {
  // 見つかった場合（z[0]を使うこと）
}
```

## 02. map

```javascript
const _ = require('lodash');
const assert = require('power-assert');

// _.map(list, iterator, [context])
// listに格納されているそれぞの要素をiterator関数に渡して実行した結果を格納した
// あらたな配列を生成する。listがオブジェクトであっても、戻り値は配列となる
// ※ listは、配列とオブジェクトどちらでも可
let double1 = function (list) {
  return _.map(list, function (x) {
    return x * x;
  });
};
let arraynise = function (row) {
  return _.map(row.split(','), function (c) {
    return c.trim();
  });
};

// Array.prototype.map(callback[, thisArg])
// 与えられた関数を配列のすべての要素に対して呼び出し、その結果からなる新しい配列を生成する
// callback = 現在の配列の要素から新しい配列の要素を生み出すための関数
// thisArg = callbackを実行するときにthisとして使用するオブジェクト
let double2 = function (arr) {
  return arr.map(function (x) {
    return x * x;
  });
};

it('is test03map', function () {
  let a = [1, 2, 3, 4, 5];
  let b = [1, 4, 9, 16, 25];
  assert(double1(a).toString() === b.toString());
  assert(double2(a).toString() === b.toString());

  let c = ' Chi, Gon, Shiro, Kiyomi ';
  let d = ['Chi', 'Gon', 'Shiro', 'Kiyomi'];
  assert(arraynise(c).toString() === d.toString());
});
```

## 03. reduce

```javascript
const _ = require('lodash');
const assert = require('power-assert');

// _.reduce(list, iterator, [memo], [context])
// listに格納されているそれぞれの要素を左から順番にiterator関数に渡して実行し、
// その結果を結合することで単一の値まで煮詰める
// memo（累積変数）は最終的に返す値の初期状態で、省略された場合はlistの最初の値が
// 初期状態となる
// listが配列の場合、memoと配列の要素がiteratorに引数として渡される
// listがオブジェクトの場合、引数はmemo, 値, キーの順番となる
// ※ listは、配列とオブジェクトどちらでも可
var lameCSV = function (str) {
  return _.reduce(
    str.split('\n'),
    function (table, row) {
      table.push(
        _.map(row.split(','), function (c) {
          return c.trim();
        })
      );
      return table;
    },
    []
  );
};

// ClipMailというPerlスクリプトをJavaScriptで再現するために、
// オブジェクトを受け取って、キーと値をイコールで結んだ
// テキストに変換する関数を作成してみた
var stringify = function (obj) {
  return _.reduce(
    obj,
    function (text, value, key) {
      if (['habit', 'hobby'].indexOf(key) < 0) {
        text += key + ' = ' + value + '\n';
      }
      return text;
    },
    ''
  );
};

it('is test04reduce', function () {
  let a = 'name,age,hair\nMerble,35,red\nBob,64,blonde';
  let b = lameCSV(a);
  assert(Array.isArray(b));
  assert(b.length === 3);
  assert(b[0].toString() === ['name', 'age', 'hair'].toString());
  assert(b[1].toString() === ['Merble', '35', 'red'].toString());
  assert(b[2].toString() === ['Bob', '64', 'blonde'].toString());

  let c = { name: 'Bob', age: 64, hair: 'blonde', habit: 'smoking' };
  let d = 'name = Bob\nage = 64\nhair = blonde\n';
  assert(stringify(c) === d);
});
```

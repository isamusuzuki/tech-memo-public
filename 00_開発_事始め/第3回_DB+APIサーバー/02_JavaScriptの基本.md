# JavaScriptの基本

作成日 2026/01/10

## 1. JavaScript、プログラミング言語としての特徴

ノンブロッキングI/O ... I/O操作を行っている間でも、他の処理がブロックされることなく進み続ける

```javascript
const fs = require('fs');

fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});

console.log('ファイルを読み込んでいます...');
```

ノンブロッキング処理の最大の利点 ... 高いパフォーマンスの実現

特に、I/O操作が多いアプリケーション（例：ウェブサーバーやリアルタイムチャットアプリなど）で効果を発揮する

## 2. コールバック関数

- 関数の引数に関数を配置する
- 結果をどう処理するのかを関数に記述して、それをまとめて送り出す

## 3. アロー関数式

```javascript
// Function式
function functionName (param1, param2) {
    statements;
}

// アロー関数式（基本匿名）
(param1, param2) => {
  statements;
};
```

コールバック関数は、アロー関数式が使われることがほとんど

アロー関数式は変形が多く、初心者を惑わせる

```javascript
// 引数が1個しかない場合、丸括弧は省略できる
param1 => {
  statemantes;
};

// 引数がない場合、丸括弧は必須
() => {
  statements;
};

// 戻り括弧とリターンは省略できる。下は { return expression } と等価
(param1, param2) => expression;

// JSオブジェクトのみを返す場合は、戻り括弧として丸括弧を使う
param1 => ({ foo: 'bar' });
```

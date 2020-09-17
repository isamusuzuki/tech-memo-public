# Node.js スクリプトを使いこなす

作成日 2020/02/10

## 01. 即時関数

無名関数をその場で実行すること\
ターミナルで、`node index.js` と叩くと、実行される

```javascript
(async () => {
  const result = await somethingPromise();
  console.log(result);
})().catch((e) => console.error(e));
```

## 02. 引数を取得する

ターミナルで、`node index.js test 123` と叩いたときの引数を取得する

```javascript
(async () => {
  // process.argv[0] => node
  // process.argv[1] => index.js
  const userName = process.argv[2];
  const userNumber = Number(process.argv[3]);
})().catch((e) => console.error(e));
```

## 03. 日付を入力する

moment をインストールする => `npm install moment --save`

```javascript
const moment = require('moment');
moment.locale('ja');
const jpnnow = moment().format('YYYY/MM/DD HH:mm:ss');
const filename = moment().format('YYYYMMDD_HHmmss');
```

## 04. 経過時間を計測する

```javascript
const start_ms = new Date().getTime();
// 実行コード
const elapsed_ms = new Date().getTime() - start_ms;
result['elapsed_ms'] = `${elapsed_ms}ms`;
```

## 05. ファイルに書き込む

Node.js v10 からサポートされたプロミスを使う

```javascript
const fs = require('fs').promises;

(async () => {
  const filepath = process.argv[2];
  const result = await somethingPromise();
  await fs.writeFile(filepath, JSON.stringify({ result }, null, 2));
})().catch((e) => console.error(e));
```

## 06. 数字をゼロ埋めする

配列の `slice()` メソッドにマイナスを入れると、最後の要素を取り出せる。これを利用する

```javascript
let num = 5;
let num_txt = ('0' + num).slice(-2);
console.log(num_txt); // => 05
```

## 07. 配列の map メソッドで、async/await を使う

1. map メソッドで呼び出す関数には、`async`をつける
1. これで、その関数の中では、`await`が使える
1. map メソッド全体を、`Promise.all()`で囲む
1. `Promise.all()`に`await`をつけて、結果の配列に入れる

```javascript
const result_list = await Promise.all(
  param_list.map(async param => {
    const result = await doSomethingPromise(param);
    return result;
  });
);
```

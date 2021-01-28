# JavaScript(ES6) のキソ

作成日 2019/04/15

## 01. var 宣言、let 宣言、const 宣言

```javascript
// 再代入オーケー、再宣言オーケー
var spam = 12;
var spam = 13;

// 再代入オーケー、再宣言NG、変数がブロックスコープでのみ有効になる
let ham = 15;
ham = 17;
let ham = 12; //=> Error

// 再代入NG、再宣言NG
const ham = 15;
ham = 17; //=> Error
```

var 宣言は忘れる

デフォルトで const 宣言を使う、それでは問題があるときだけ let 宣言を使う

## 02. JS オブジェクト

`{key: value}`で表されるデータ。PHP でいうところの連想配列

```javascript
const person = {
  name: { firstName: 'Bob', lastName: 'Smith' },
  age: 32,
  gender: 'male',
  interests: ['music', 'skiing'],
  greeting: function () {
    alert("Hi! I'm " + this.name[0] + '.');
  },
};
```

value に置けるデータ => 数、文字列、JS オブジェクト、配列、関数

### JS オブジェクトのオプション 1

Value に関数を置くときは、`key: function() {}`以外に、`関数名 () {}`と書ける

=> JS オブジェクトのメソッドとして定義する

```javascript
const obj = {
  f: function (params) {}, //=> OK
  g(params) {}, //=> OK
};

const func1 = function () {}; // 匿名関数に名前を定義する
function func2() {} // 普通に関数を定義する
```

### JS オブジェクトのオプション 2

Value の名前とその値を表す変数とが同名である場合には、値の指定を省略できる

```javascript
const title = 'Programming 101';
const price = 3700;
const book = { title, price };
// {title: title, price: price}と同じ
```

### JS オブジェクトのオプション 3

key 名を動的に生成したいときは、ブラケットでくくることで、式の値から動的に生成できる

```javascript
let i = 0;
const data = {
  [`hoge${++i}`]: 15,
  [`hoge${++i}`]: 20,
};
```

## 03. JSON (JavaScript Object Notation)

JS オブジェクトから関数を取り除いて、文字列化したもの

```json
{
  "name": {
    "firstName": "Bob",
    "lastName": "Smith"
  },
  "age": 32,
  "gender": "male",
  "interests": ["music", "skiing"]
}
```

じつは、たったひとつの文字列や、何かの配列も、有効な JSON であったりする

わからなくなったらオンラインのバリデータでチェックするべし

=> [JSONLint \- The JSON Validator](https://jsonlint.com/)

### JS オブジェクトから JSON への変換

```javascript
const string1 = JSON.stringify(object1);
//=> 改行なしの文字列にする
const string2 = JSON.stringify(object2, null, 2);
//=> 改行とインデントがある見やすい文字列にする
```

### JSON から JS オブジェクトへの変換

```javascript
const object1 = JSON.parse(string1);
```

## 04. アロー関数式

function 式に比べてより短い構文を持ち、常に匿名関数である

```javascript
// 基本形
(param1, param2) => {
  statements;
};

// 引数が1個しかない場合、丸括弧は任意
(param1) => {
  statemantes;
};

// 引数がない場合、丸括弧は必須
() => {
  statements;
};

// 戻り括弧とリターンは省略できる。下は { return expression } と等価
(param1, param2) => expression;

// JSオブジェクトのみを返す場合は、戻り括弧として丸括弧を使う
param1 = { foo: 'bar' };
```

### アロー関数の中では、this が使えない

アロー関数はオールマイティではない、今後も function 式は使う

```javascript
// これは問題なく動作する
const profile1 = {
  name: 'Alex',
  getName: function () {
    return this.name;
  },
};
profile1.getName(); // => Alex

// これはエラーが発生する
const profile2 = {
  name: 'Bob',
  getName: () => {
    return this.name;
  },
};
profile2.getName();
// => TypeError: Cannot read property of 'name' of undefined
```

## 05. テンプレート文字列

テンプレート文字列はシングルクォート、ダブルクォートの代わりに、アクサングラーブで文字列を括る

その中では、`${expression}`の形で、変数（式）を文字列の中に埋め込むことができる

これまでは、変数とリテラルを＋演算子で連結していくしかなかったので、ぐんとシンプルになった

```javascript
const a = `1 + 2 = ${1 + 2}`;
console.log(a); //=> 1 + 2 = 3
```

## 06. Promise

JavaScript は「ノンブロッキング I/O」であり、関数を呼んだ時にそのレスポンスを待たずに、次の行へと進んでいく。axios による外部サーバーへのアクセスなど、処理にものすごく時間がかかる関数の場合は、結果が得られたときに実行するコールバック関数を、引数として与えることで対処する。成功したときのコールバック関数と、失敗したときのコールバック関数を、なるたけ自然に書けるようにしたのが Promise である

### Promise を設定する側（時間がかかる関数）

- 新規の Promise インスタンスを返すように書き、
- そのインスタンスの引数として関数を書き、
- その関数の引数として resolve と reject という関数を与え、
- 関数の本体で、本当にやりたいことを書き、
- 正常終了したときは resolve 関数を実行し、
- 異常終了したときは reject 関数を実行する

```javascript
// authモジュールのgetTokenアクション関数
getToken (context, payload) {
    return new Promise((resolve, reject) => {
        axios.post(`${context.rootState.settings.serverUrl}/kaigi/auth`,
            {login_name: payload.account, login_password: payload.password}
        ).then(response => {
            // 成功
            const newAuth = {name: payload.account, token: response.data.token}
            context.commit('update', newAuth)
            localStorage.setItem('kaigi-auth', JSON.stringify(newAuth))
            resolve()
        }).catch(error => {
            // 失敗
            reject(error.response)
        });
    });
}
```

### Promise を使う側

- `.then()`でチェーンできる、その中身は関数
- この関数の引数は、resolve 関数に与えられた引数
- この関数の本体は、必ず getToken アクション関数が正常終了したときに実行される
- `.catch()`でチェーンできる、その中身も関数
- この関数の引数は、reject 関数に与えられた引数
- この関数の本体は、必ず getToken アクション関数が異常終了したときに実行される

```javascript
// loginページのlogin()メソッド
this.$store
  .dispatch('auth/getToken', {
    account: this.loginName,
    password: this.password,
  })
  .then(() => {
    this.$store.dispatch('loader/off');
    this.$router.push({ path: '/home' });
  })
  .catch((error) => {
    this.$store.dispatch('message/error', { messages: ['ログイン失敗'] });
    this.$store.dispatch('loader/off');
  });
```

## 07. JS モジュール

[JavaScript modules via script tag](https://caniuse.com/#feat=es6-module)

> Loading JavaScript module scripts using `<script type="module">` Includes support for the nomodule attribute.

script タグに type 属性を追加することで、import/export 構文が使えるようになる機能

index.html

```html
<script type="module">
  import Auth from '/modules/auth';
</script>
```

modules/auth.js

```javascript
export default {
    ...
}
```

# import と require

作成日 2019/12/02

## 01. import と require の違いを整理する

### require 構文のサンプルコード

-   CommonJS の仕様
-   Node.js でサポートしている書き方
-   ブラウザで実行する場合は、コンパイルが必要

`index.js`ファイル

```javascript
const module = require('./module.js');
console.log(module('Taro'));
```

`module.js`ファイル

```javascript
module.exports = name => `Hello ${name}`;
```

### module 構文(import)のサンプルコード

-   ES2015 から利用できる
-   オプションをつければブラウザでも利用可能

`index.js`ファイル

```javascript
import { hello, goodbye } from './module';
console.log(hello('Taro'));
console.log(goodbye('Taro'));
```

`module.js`ファイル

```javascript
export const hello = name => `Hello ${name}`;
export const goodbye = name => `Goodbye ${name}`;
```

### 参考記事

[JavaScript ｜モジュール化\(import と require の違い\) \- わくわく Bank](https://www.wakuwakubank.com/posts/466-javascript-module-import-export/)

[イマドキの npm では何を配布すべきか \- Qiita](https://qiita.com/zprodev/items/e2f7e24acf0ca1227e13)

## 02. Node.js で import を使う方法

-   export では名前を付与する方法と default で付与しない方法がある
-   import / export を使う場合はファイルの拡張子を「.mjs」にする
-   node コマンドで実行する際はオプションを付与する

```bash
node --experimental-modules index.mjs
```

### 参考記事

[【Node\.js 入門】初心者でも分かる import / export でモジュールを使う方法まとめ！ \| 侍エンジニア塾ブログ（Samurai Blog） \- プログラミング入門者向けサイト](https://www.sejuku.net/blog/86754)

[ES modules in Node\.js 12, from experimental to release \- LogRocket Blog](https://blog.logrocket.com/es-modules-in-node-js-12-from-experimental-to-release/)

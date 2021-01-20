# TypeScript コードを単体テストする

作成日 2021/01/17、更新日 2021/01/19

## 01. 必要なモジュールをインストールする

```bash
npm install --save-dev mocha @types/mocha
npm install --save-dev power-assert @types/power-assert
npm install --save-dev intelli-espower-loader
npm install --save-dev ts-node
```

- Mocha ... JavaScript のテストフレームワーク
- power-assert ... Node.js に標準でついている assert を強力にしたもの
- intelli-espower-loader ... power-assert の結果を表示するためのもの
- ts-node ... TypeScript のコンパイルと実行を同時に行うためのもの

## 02. TypeScriptでテストコードを書く

tests/test.ts

```javascript
import { describe, it } from 'mocha';
import assert from 'power-assert';

describe('Array#join', () => {
  it('joins all elements into a string with separator', () => {
    assert(['a', 'b', 'c'].join(':') === 'a:b:c:');
  });
});
```

わざと失敗するコードを書いている

## 03. 単体テストを実行する

```bash
npx mocha --version
# => 8.2.1

npx mocha --require ts-node/register --require intelli-espower-loader tests/**/*.ts
```

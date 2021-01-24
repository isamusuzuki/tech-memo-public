# Jest を導入して、TypeScript コードをテストする

作成日 2021/01/21

## 01. 必要なモジュールをインストールする

```bash
npm install --save-dev jest @types/jest ts-jest
```

## 02. package.json に Jest の設定を書き込む

定型であるので、なにひとついじらずこのままコピペする

```json
{
  "jest": {
    "moduleFileExtensions": ["ts", "js"],
    "transform": {
      "^.+\\.ts$": "ts-jest"
    },
    "globals": {
      "ts-jest": {
        "tsconfig": "tsconfig.json"
      }
    },
    "testMatch": ["**/tests/**/*.test.ts"]
  }
}
```

## 03. TypeScript でテストコードを書く

tests/array.test.ts

```javascript
describe('Array#join', () => {
  test('joins all elements into a string with separator', () => {
    expect(['a', 'b', 'c'].join(':')).toBe('a:b:c:');
  });
});
```

わざと失敗するコードを書いている

## 03. 単体テストを実行する

package.json の script 項目に以下のように書く

```json
{
  "scripts": {
    "test": "jest"
}
```

npm コマンドで走らせる

```bash
npm run test
```

npx コマンドを使えば、package.json に書かなくても実行できる

```bash
npx jest
```

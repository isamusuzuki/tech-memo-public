# eslint-config-prettier

作成日 2025/09/24、更新日 2025/09/25

## 1. 公式サイト（英語）を読む

[eslint-config-prettier - npm](https://www.npmjs.com/package/eslint-config-prettier)

「Prettier」と競合したり、不要になったりするすべてのルールを無効にします。これにより、お好みの共有設定を使用しながら、Prettier使用時のスタイルの選択が邪魔にならないようにできます。この設定はルールを無効にするだけなので、他の設定と組み合わせて使用することのみ意味があります。

### インストール

```bash
npm i -D eslint-config-prettier
```

### 設定

```javascript
import someConfig from "some-other-config-you-use";
import eslintConfigPrettier from "eslint-config-prettier/flat";

export default [
  someConfig,
  eslintConfigPrettier,
];
```

## 2. 具体的には、どんな働きをしているのか？

[eslint-config-prettier - Code](https://www.npmjs.com/package/eslint-config-prettier?activeTab=code)

- index.jsを開く
- semiを検索すると、20個のセミコロン関連のルールがオフにされていることがわかる
- indentを検索すると、16個のインデント関連のルールがオフにされていることがわかる

Prettierで設定するべき項目については、ESLintがしゃしゃりでてこないようにしていることがわかった

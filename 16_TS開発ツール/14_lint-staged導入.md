# lint-staged導入

作成日 2025/09/17、更新日 2025/09/18

## 1. 解説記事を読む

[コードを綺麗に保ちたい？それhuskyでできるよ！](https://qiita.com/mu-suke08/items/43a492fda5cd71a31506)

Huskyの設定（frontend/.husky/pre-commit）

```bash
npx lint-staged
```

lint-stagedの設定例（package.jsonに追加）

```json
{
  "scripts": {
    "lint:precommit": "eslint 'src/**/*.{ts,tsx}' --max-warnings 0",
    "fmt:precommit": "prettier -l './**/*.{js,jsx,ts,tsx,json,css,scss}'",
  },
  "lint-staged": {
    "src/**/*.{ts,tsx}": "npm run lint:precommit",
    "src/**/*.{js,jsx,ts,tsx,json,css,scss}": "npm run fmt:precommit"
  },
}
```

## 2. 公式サイト（英語）を読む

[lint-staged - npm](https://www.npmjs.com/package/lint-staged)

> Run tasks like formatters and linters against staged git files and don't let 💩 slip into your code base!

Gitのステージされたファイルに対してフォーマッターやリンターのようなタスクを走らせて、コードベースにウンコを紛れこませないようにする

インストール => `npm install --save-dev lint-staged`

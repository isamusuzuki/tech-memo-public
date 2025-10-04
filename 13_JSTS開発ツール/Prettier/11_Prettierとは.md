# Prettierとは

作成日 2025/09/29

## 1. 公式サイト（英語）を読む

[Prettier · Opinionated Code Formatter · Prettier](https://prettier.io/)

Opinionated = 独断的な

## 2. インストール

```bash
npm install --save-dev --save-exact prettier

bun add --dev --exact prettier
```

## 2. `.prettierrc.json`ファイルの設定例

```json
{
    "trailingComma": "es5",
    "tabWidth": 4,
    "semi": true,
    "singleQuote": true,
    "printWidth": 140
}
```

## 3. `.prettierignore`ファイルの設定例

```text
package-lock.json

bun.lock
```

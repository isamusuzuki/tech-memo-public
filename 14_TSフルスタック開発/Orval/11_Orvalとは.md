# Orvalとは？

作成日 2025/09/02

## 1. 公式サイト（英語）を読む

[orval - Restful client generator](https://orval.dev/)

OpenAPIスキーマから、TypeScriptのAPIクライアントを自動生成するツール

インストール => `npm i orval -D`

## 1a. [Quick Start](https://orval.dev/quick-start)

orval.config.js

```javascript
module.exports = {
  'petstore-file': {
    input: './petstore.yaml',
    output: './src/petstore.ts',
  },
};
```

```bash
orval --config ./orval.config.js
```

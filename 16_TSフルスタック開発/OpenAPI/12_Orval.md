# Orval

作成日 2025/09/02、更新日 2025/11/11

## 1. 公式サイト（英語）を読む

[orval - Restful client generator](https://orval.dev/)

OpenAPIスキーマから、TypeScriptのAPIクライアントを自動生成するツール

インストール => `npm i orval -D`

[Quick Start](https://orval.dev/quick-start)を読む

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

## 2. 解説記事aを読む

[【Orval】APIスキーマからほぼノーコードでAPI/クライアント/MCPを生成する](https://zenn.dev/hiromoa1/articles/65756e296283e6)

ファイル＆フォルダ構造

```text
--sample-project/
    |--app/
    |   |--api/
    |   |   `--route.ts
    |   `--page.tsx
    |--client/
    |   `--api.ts
    |--endpoints/
    |   `--validator.ts
    |--node_modules/
    |--schemas/
    |   `--index.ts
    |--orval.config.ts
    |--package-lokc.json
    |--package.json
    |--schemas.yaml
    `--tsconfig.json
```

orval.config.ts

```javascript
import { defineConfig } from "orval";

export default defineConfig({
    api: {
        input: "schemas.yaml"
        output: {
            target: "endpoints/",
            schemas: "schemas/",
            client: "hono"
        }
    },
    client: {
        output: {
            target: "client/",
            schemas: "schemas/",
            client: "fetch",
            baseUrl: "http://localhost:3000/api"
        }
    }
})
```

pacakge.json

```json
{
    "scripts": {
        "generate": "orval --config ./orval.config.ts"
    }
}
```

## 3. 解説記事bを読む

[TypeSpec、Orval、Storybook を使ってフロントエンドのモック生成を自動化する](https://zenn.dev/osushioichii/articles/63ace4f19ba2b9)

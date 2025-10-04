# 開発コンテナ設定例 typescript+bun編

作成日 2025/10/01

## 1. ファイル＆フォルダ構成

```text
--.devcontainer/
    |--devcontainer.json
    `--Dockerfile
```

## devcontainer.json

```json
{
"name": "bun",
  "build": {
    "dockerfile": "./Dockerfile"
  },
  "customizations": {
    "vsocode": {
      "extensions": [
        "dbaeumer.vscode-eslint",
        "editorconfig.editorconfig",
        "esbenp.prettier-vscode"
      ],
    "settings": {
      "editor.defaultFormatter": "esbenp.prettier-vscode",
      "editor.formatOnSave": true,
      "editor.codeActionsOnSave": {
        "source.fixAll.eslint": "explicit",
        "source.organizeImports": "explicit"
        },
      },
    }
  }
}
```

## 3. Dockerfile

```bash
FROM oven/bun:1.2.23-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    && rm -rf /var/lib/apt/lists/*

# タイムゾーンをJSTに変更する
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

USER bun
```

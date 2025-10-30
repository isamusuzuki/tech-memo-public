# 開発コンテナ 設定例 bun

作成日 2025/10/01、更新日 2025/10/30

## 1. ファイル＆フォルダ構成

```text
--.devcontainer/
    |--devcontainer.json
    `--Dockerfile
```

## 2. devcontainer.json

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
FROM oven/bun:1.3-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    && rm -rf /var/lib/apt/lists/*

# タイムゾーンをJSTに変更する
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

USER bun
```

## 4. Dockerイメージ調査

[oven/bun - Docker Image | Docker Hub](https://hub.docker.com/r/oven/bun)

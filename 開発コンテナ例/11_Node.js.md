# 開発コンテナ例 Node.js

作成日 2025/10/30、更新日 2025/12/10

## .devcontainer/devcontainer.json

```json
{
    "name": "Node.js",
    "build": {
        "dockerfile": "./Dockerfile"
    },
    "customizations": {
        "vscode": {
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
                }
            }
        }
    }
}
```

## .devcontainer/Dockerfile

```bash
FROM node:24-trixie-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    && rm -rf /var/lib/apt/lists/*

# タイムゾーンをJSTに変更する
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# npmを最新に更新
RUN npm install -g npm@latest

USER node
```

## Dockerイメージ調査

[node - Official Image | Docker Hub](https://hub.docker.com/_/node)

[docker-node/24/trixie-slim/Dockerfile](https://github.com/nodejs/docker-node/blob/a364e16a23fb97ea9768e5adbae36f1de63f44e9/24/trixie-slim/Dockerfile)

nodeユーザーを使う証拠を発見

```bash
# 4行目
RUN useradd --uid 1000 --gid node --shell /bin/bash --create-home node
```

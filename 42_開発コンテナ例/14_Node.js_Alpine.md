# 開発コンテナ例 Node.js Alpine

作成日 2025/11/05、更新日 2025/12/10

## .devcontainer/devcontainer.json

```json
{
    "name": "Alpine",
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
FROM node:22-alpine

RUN apk update && apk add --no-cache \
    git \
    postgresql16-client \
    tzdata \
    && rm -rf /var/cache/apk/*

# タイムゾーンをJSTに変更する
RUN cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# npmを最新に更新
RUN npm install -g npm@latest

USER node
```

## 4. Dockerイメージ調査

[node - Official Image | Docker Hub](https://hub.docker.com/_/node)

[docker-node/22/alpine3.22/Dockerfile](https://github.com/nodejs/docker-node/blob/bf78d7603fbea92cd3652edb3b2edadd6f5a3fe8/22/alpine3.22/Dockerfile)

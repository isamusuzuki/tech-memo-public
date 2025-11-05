# 開発コンテナ 設定例 Alpine

作成日 2025/11/05

## 1. ファイル＆フォルダ構成

```text
--.devcontainer/
    |--devcontainer.json
    `--Dockerfile
```

## 2. devcontainer.json

```json
{
    "name": "Alpine",
    "build": {
        "dockerfile": "./Dockerfile"
    },
    "customizations": {
        "vscode": {
            "extensions": ["dbaeumer.vscode-eslint", "editorconfig.editorconfig", "esbenp.prettier-vscode"],
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

## 3. Dockerfile

```bash
FROM node:22-alpine

USER node
```

## 4. Dockerイメージ調査

[node - Official Image | Docker Hub](https://hub.docker.com/_/node)

[docker-node/22/alpine3.22/Dockerfile](https://github.com/nodejs/docker-node/blob/bf78d7603fbea92cd3652edb3b2edadd6f5a3fe8/22/alpine3.22/Dockerfile)

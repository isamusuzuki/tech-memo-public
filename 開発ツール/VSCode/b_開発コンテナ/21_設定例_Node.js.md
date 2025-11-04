# 開発コンテナ 設定例 Node.js

作成日 2025/10/30

## 1. ファイル＆フォルダ構成

```text
--.devcontainer/
    |--devcontainer.json
    `--Dockerfile
```

## 2. devcontainer.json

```json
{
"name": "Node.js",
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
FROM node:22-bookworm-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    locales \
    postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# ロケールをja_JP.UTF-8に変更する
RUN sed -i -E 's/# (ja_JP.UTF-8)/\1/' /etc/locale.gen \
  && locale-gen \
  && update-locale LANG=ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8

# タイムゾーンをJSTに変更する
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# npmを最新に更新
RUN npm install -g npm@latest

USER node
```

## 4. Dockerイメージ調査

[node - Official Image | Docker Hub](https://hub.docker.com/_/node)

[docker-node/22/bookworm-slim/Dockerfile](https://github.com/nodejs/docker-node/blob/bf78d7603fbea92cd3652edb3b2edadd6f5a3fe8/22/bookworm-slim/Dockerfile)

```bash
# 4行目
RUN useradd --uid 1000 --gid node --shell /bin/bash --create-home node
```

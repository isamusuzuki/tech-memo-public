# 開発コンテナ例 bun

作成日 2025/10/01、更新日 2026/01/17

## 1. ファイル＆ディレクトリ構成

```text
--{project}/
    `--.devcontainer/
        |--devcontainer.json
        `--Dockerfile
```

## 2. devcontainer.json

### Biomeを利用する場合

```json
{
    "name": "bun",
    "build": {
        "dockerfile": "./Dockerfile"
    },
    "customizations": {
        "vscode": {
            "extensions": [
                "biomejs.biome"
            ],
            "settings": {
                "editor.defaultFormatter": "biomejs.biome",
                "editor.formatOnSave": true,
                "editor.codeActionsOnSave": {
                    "quickfix.biome": "explicit",
                    "source.fixAll.biome": "explicit",
                    "source.sortImports.biome": "explicit"
                }
            }
        }
    }
}
```

### ESLint+Prettierを利用する場合

```json
{
"name": "bun",
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

### Dockerイメージ調査

[oven/bun - Docker Image | Docker Hub](https://hub.docker.com/r/oven/bun)

Variants = [debian, slim, alpine, distroless]

[bun/dockerhub/debian-slim/Dockerfile](https://github.com/oven-sh/bun/blob/main/dockerhub/debian-slim/Dockerfile)

bunユーザーを使う証拠を発見

```bash
# 76行目
RUN useradd bun --uid 1000 --gid bun --shell /bin/sh --create-home
```

bunユーザーは、デフォルトのシェルがbashではなくてshであった

ターミナルで`bash`と叩けば、bashに変更することができる

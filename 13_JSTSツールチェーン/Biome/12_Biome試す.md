# Biomeを試す

作成日 2026/01/13

## 1. 既存プロジェクトにインストールする

```bash
npm install --save-dev --save-exact @biomejs/biome

npx @biomejs/biome init
```

## 2. package.jsonのscripts項目に追記する

```json
{
    "scripts": {
        "lint": "biome lint --write ./src",
        "format": "biome format --write ./src"
    }
}
```

スクリプトを実行する

```bash
npm run lint

npm run format
```

## 3. 開発コンテナを設定する

.devcontainer/devcontainer.json

```bash
{
    "name": "Node.js",
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

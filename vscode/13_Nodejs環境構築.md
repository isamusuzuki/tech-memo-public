# vscode の Node.js 環境構築

作成日 2019/11/27

## 01. 新規プロジェクトの作成

Node.js はインストール済みであるとする

```bash
# Git Bashを開く
cd ~/YOUR-PROJECT

# Node.jsのバージョン番号を確認する
node -v
# => v12.13.0
npm -v
# => 6.12.0

# package.jsonファイルを生成する
npm init -y
```

出来上がった`package.json`ファイル

```json
{
  "name": "YOUR-PROJECT",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "略"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "略"
  },
  "homepage": "略",
  "dependencies": {}
}
```

## 02. 必須の拡張機能

[Prettier \- Code formatter \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

JavaScript を自動で修正・整形してくれる便利ツールだが、
ときに整形ルールを変更したいときもある

`.prettierrc`ファイル

```json
{
  "trailingComma": "es5",
  "tabWidth": 4,
  "semi": true,
  "singleQuote": true
}
```

## 03. require 構文に、サジェスチョンが表示するのを止める

以下のようなサジェスチョンが、require 構文に表示される

`File is a CommonJS module; It may be converted to an ES6 module.`

Node.js スクリプトで、標準的に import/export が使えるようになるのは、
まだ先の話であるため、vscode の設定をいじって、サジェスチョンを出さないようにする

`.vscode/settings.json`ファイル

```json
{
  "javascript.suggestionActions.enabled": false
}
```

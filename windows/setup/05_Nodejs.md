# vscode の Node.js 環境構築

作成日 2019/11/27、更新日 2020/02/21

## 01. Node.js をインストールする

Windows の場合は、Chocolatey を使ってインストールする => `choco install nodejs-lts`

[Chocolatey Software \| Node\.js LTS \(Install\) 12\.14\.1](https://chocolatey.org/packages/nodejs-lts)

```bash
# Node.jsのバージョン番号を確認する
node -v
# => v12.13.0
npm -v
# => 6.12.0
```

## 02. 新規プロジェクトを作成する

Git Bash を開く

```bash
cd ~/YOUR-PROJECT

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

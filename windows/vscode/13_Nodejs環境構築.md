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

Git Bashを開く

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

## 03. Prettier は、必須の vscode 拡張機能

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

### 設定の上書き

MarkdownファイルもPrettierが整形してくれるのは助かっているが、タブ幅がリストインデントに影響しているに困っていた\
ある条件の場合のみ、設定が上書きされる方法を発見した

[Configuration File · Prettier](https://prettier.io/docs/en/configuration.html)

```json
{
    "trailingComma": "es5",
    "tabWidth": 4,
    "semi": true,
    "singleQuote": true,
    "overrides": [
        {
            "files": "*.md",
            "options": {
                "tabWidth": 2
            }
        }
    ]
}
```

## 04. require 構文に、サジェスチョンが表示するのを止める

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

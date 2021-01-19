# ESLint を導入する

作成日 2021/01/17

## 01. ESLint をインストールする

```bash
# 作成済みのプロジェクトに行く
cd ~/avocado

# ESLint をインストールする
npm install --save-dev eslint

# バージョン番号を確認する
npx eslint --version
# => v7.18.0
```

## 02. 初期設定を行う

```bash
npx eslint --init
```

ここで聞かれる質問とその選択肢は以下の通り

```text
? How would you like to use ESLint?
  To check syntax only
> To check syntax and find problems
  To check syntax, find problems, and enforce code style

? What type of modules does your project use?
> JavaScript modules (import/export)
  CommonJS (require/exports)
  Non of these

? Which framework does your project use?
  React
  Vue.js
> None of these

? Does your project use TypeScript?
  No
> Yes

? Where does your code run?
> Browser
> Node

? What format do you want your config file to be in?
  JavaScript
  YAML
> JSON

? Would you like to install them now with npm?
  No
> Yes
```

印がついている選択肢を選んで出来上がる .eslintrc.json が以下の通り

```json
{
    "env": {
        "browser": true,
        "es2021": true,
        "node": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": 12,
        "sourceType": "module"
    },
    "plugins": [
        "@typescript-eslint"
    ],
    "rules": {
    }
}
```

あとは、Visual Studio Code に `ESLint` という機能拡張をインストールしてあれば、特にコマンドを叩かなくても、自動的に問題点を指摘してくれる

## 03. 追加の設定

TypeScriptファイルしかチェックさせるつもりがないので、生成したJavaScriptファイルについては、ESLintに無視させたい

解決方法: `.eslintignore` ファイルを作成して、その中に無視させたいファイル名を書く

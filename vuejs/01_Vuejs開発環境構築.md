# Vue.js の開発環境を構築する

作成日 2022/12/15

## 新規プロジェクト作成

- まっさらな状態から開発環境を構築する
- Node.jsはインストールされているものとする
- フォルダ名の `avocado` は適当
- `npm i -D {name}` は、`npm install --save-dev {name}` の短縮形

```bash
node -v
# => v18.12.1
npm -v
# => 9.2.0

# 新規プロジェクトを作成する
cd ~
mkdir avocado
cd avocado

# package.json ファイルを生成する
npm init -y
```

## TypeScript 導入

```bash
npm i -D typescript
npx tsc --init
# => tsconfig.json ファイルが生成される
```

### tsconfig.json の設定例

```json
{
  "compilerOptions": {
    "target": "ES2019",
    "module": "ES2015",
    "moduleResolution": "node",
    "sourceMap": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "noImplicitReturns": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*.ts", "src/**/*.vue"]
}
```

## ESLint 導入

※ 前提として、VSCode の拡張機能としての ESLint はインストールしない

```bash
npm i -D eslint
npm i -D @eslint/create-config
npx eslint --init
# => インタラクティブなセットアップが始まる
# => 回答が終わるとモジュールの追加インストールが始まり、
# => .eslintrc.json ファイルが生成される
```

### ESLint セットアップの回答例

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
> Vue.js
  None of these

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

? Would you like to install them now?
  No
> Yes

? Which package manager do you want to user?
> npm
  yarn
  pnpm
```

## Webpack 導入

```bash
npm i -D webpack webpack-cli webpack-dev-server
npm i -D ts-loader
npm i -D css-loader style-loader
```

## Vue.js 導入

```bash
npm i -D vue
npm i -D vue-loader
npm i -D @vue/compiler-sfc
```

※ SFC = Single File Components 単一ファイルコンポーネント

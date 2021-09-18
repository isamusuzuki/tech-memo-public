# Vue.js 3 の開発環境を構築する その 1

作成日 2021/09/17

## 01. Vue.js 2x から 3.x への移行

今まで、TypeScript + Webpack + Vue.js 2.x の組み合わせで、Web クライアントを開発してきたが、Vue.js 3.x へ移行することにした

## 02. Vue.js のバージョン移行に影響されない部分の構築

- まっさらな状態から開発環境を構築する
- `npm i -D` は、`npm install --save-dev` の短縮形

```bash
node -v
# => v14.17.6
npm -v
# => 7.23.0

# 新規プロジェクトを作成する
cd ~
mkdir avocado
cd avocado

# package.jsonファイルを生成する
npm init -y
```

### TypeScript 導入

```bash
npm i -D typescript
npx tsc --init
# => tsconfig.jsonが生成される
```

### tsconfig.json の設定例

```json
{
  "compilerOptions": {
    "target": "ES2019",
    "module": "es2015",
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

### ESLint 導入

```bash
npm i -D eslint
npx eslint --init
# => インタラクティブなセットアップが始まる
# => 回答が終わるとモジュールの追加インストールが始まり、
# => .eslintrc.json ファイルが生成される
```

### セットアップの回答例

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

? Would you like to install them now with npm?
  No
> Yes
```

### Webpack 導入

```bash
npm i -D webpack webpack-cli webpack-dev-server
npm i -D ts-loader
npm i -D css-loader style-loader
```

## 03. Vue.js 3.x と Webpack の組み合わせで開発するための構築

```bash
# Vue.js 3.x 導入
npm i -D vue@next

# Webpack ローダー導入
npm i -D vue-loader@next
npm i -D @vue/compiler-sfc
```

- `vue-loader@next` は、`vue-loader` の置き換え
- `@vue/compiler-sfc` は、`vue-template-loader` の置き換え

### webpack.config.js 設定例

```javascript
const path = require('path')
const { VueLoaderPlugin } = require('vue-loader')

module.exports = {
    mode: 'development',
    entry: {
        gyomu: './src/index.ts'
    },
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'public/static')
    },
    devServer: {
        static: {
            directory: path.resolve(__dirname, 'public')
        },
        compress: true,
        port: 8080,
    },
    module: {
        rules: [
            {
                test: /\.vue$/,
                loader: 'vue-loader',
            },
            {
                test: /\.ts$/,
                loader: 'ts-loader',
                exclude: /node_modules/,
                options: {
                    appendTsSuffixTo: [/\.vue$/],
                },
            },
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader'],
            },
        ],
    },
    resolve: {
        extensions: ['.js', '.ts', '.vue'],
    },
    plugins: [
        new VueLoaderPlugin()
    ]
}
```

# TypeScript を導入する

作成日 2021/01/17、更新日 2021/02/21

## 01. TypeScript をインストールする

```bash
# 新規プロジェクトを作成する
cd ~
mkdir avocado
cd avocado

# package.jsonファイルを生成する
npm init -y

# TypeScriptをインストールする
npm install --save-dev typescript

# バージョン番号を確認する
npx tsc --version
# => Version 4.1.3

# tsconfig.jsonファイルを生成する
npx tsc --init

# コンパイルする
npx tsc
```

## 02. tsconfig.json ファイルをいじる

生成したばかりの、なにもいじっていない状態は、以下の通り

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

### tsconfig.json ファイル 変更点 1

この中で変更すべき項目は `target` である。ECMAScript5(ES5) は、Internet Explorer 11 でも動作させられるほどの古い規格である。ターゲットブラウザが Google Chorme ならば、"ES2017" をチョイスできる

### tsconfig.json ファイル 変更点 2

自分の好みとしては、TypeScript ファイルは、すべて`src`フォルダの中にまとめて置き、生成された JavaScript ファイルは、プロジェクトのルートフォルダ以下に、フォルダ構造を保ったまま配置されるようにしたい

```text
--avocado/
    |--src/
    |   |--static/
    |   |   `---index.ts ... コンパイル前1
    |   `--main.ts       ... コンパイル前2
    |
    |--static/
    |   `--index.js      ... コンパイル後1
    `--main.js           ... コンパイル後2
```

これは、`include` と `outDir` の 2 つの項目を追加することで実現できる

### tsconfig.json ファイル 変更点 3

`sourceMap` 項目を追加して、ソースマップを出力させる

### tsconfig.json ファイル 変更点 まとめ

```json
{
  "include": ["src/**/*"],
  "compilerOptions": {
    "target": "ES2017",
    "module": "commonjs",
    "sourceMap": true,
    "outDir": "",
    "strict": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

## 03. TypeScriptを直接実行する

ts-node を使うと、Typescriptのコードのまま、実行できる

```bash
npm i -D ts-node
```

ファイルの配置を変更する

```text
--avocado/
    |--src/
    |   `--main.ts  ... ここにあったものを
    |
    |--main.ts      ... 直接ここに置く
    `--(main.js)    ... コンパイルしていたものは削除
```

```bash
# 今まで実行していたコードを
node main.js

# ts-nodeを使ったものに変更する
ts-node main.ts
```

# TypeScript を導入する

作成日 2021/01/17、更新日 2021/05/12

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

これを以下のように変更した

```json
{
  "include": ["src/**/*"],
  "compilerOptions": {
    "target": "ES2019",
    "module": "es2015",
    "sourceMap": true,
    "outDir": "",
    "strict": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

### 変更点 1: target

この中で変更すべき項目は `target` である。ECMAScript5(ES5) は、Internet Explorer 11 でも動作させられるほどの古い規格である。ターゲットブラウザが Google Chorme ならば、"ES2019" をチョイスできる

### 変更点 2: include, outDir

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

### 変更点 3: sourceMap

`sourceMap` 項目を追加して、ソースマップを出力させる

# TypeScript を導入する

作成日 2021/01/17

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
```

## 02. tsconfig.json ファイルの中身を確認する

なにもいじっていないデフォルトの状態は以下の通り

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

この中で変更すべきは `target` である。ECMAScript5(ES5) は、Internet Explorer 11 でも動作させられるほどの古い規格である。ターゲットブラウザが Google Chorme をならば、"ES2017" をチョイスできる

自分の好みとしては、TypeScript ファイルは、すべて`src`フォルダの中にまとめて置き、
生成された JavaScript ファイルは、プロジェクトのルートフォルダ以下に散らばるように配置したい

```text
--avocado/
    |--src/
    |   |--static/
    |   |   `---index.ts
    |   `--main.ts
    |
    |--static/
    |   `--index.js
    `--main.js
```

`target` を変更し、`include` と `outDir` の 2 つを追加した後が、以下の通り

```json
{
  "include": ["src/**/*"],
  "compilerOptions": {
    "target": "ES2017",
    "module": "commonjs",
    "outDir": "",
    "strict": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

## 03. よく使うコマンド

```bash
# コンパイル
npx tsc

# スクリプトの実行
node main.js
```

### コンパイルと実行を同時に行うには

ts-node をインストールすれば実現できる

インストール => `npm install --save-dev ts-node`

```bash
npx ts-node src/main.ts
```

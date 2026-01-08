# Node.jsの基本

作成日 2026/01/08

[Node.js](https://nodejs.org/ja)とは、ブラウザに搭載されているV8エンジンというJavaScript実行環境を独立させて、OSの上で、JavaScriptコードを実行できるようにしたもの

[npm](https://www.npmjs.com/)は、Node Package Managerの略で、JavaScript向けのパッケージを組み込むためのツール

package.jsonは、npmの設定ファイル

```bash
node -v
# v24.12.0

npm -v
# 11.7.0

# 開発プロジェクトを開始する（あまり利用しなくなった）
npm init -y
# package.jsonが生成される

# ツールを利用して開発プロジェクトを開始する
npm create {package-name}
# ツールが出す質問に答える
# package.jsonを含むさまさまなファイルが生成される

# パッケージをインストールする
npm install {package-name}
```

## package.jsonの例

```json
{
    "name": "first-app",
    "version": "0.0.0",
    "private": true,
    "scripts": {
        "deploy": "wrangler deploy",
        "dev": "wrangler dev",
        "start": "wrangler dev"
    },
    "devDependencies": {
        "wrangler": "^4.54.0"
    }
}
```

- "name","version","private"設定 ... この開発プロジェクトに関する説明
- "scripts"設定 ... `npm run {command}`でそのコマンドを実行できる
- ("dependencies"),"devDependencies"設定 ... このプロジェクトを動かすのに必要な追加パッケージは何かを示す

```bash
# 開発サーバーを起動する
npm run dev
# 実際には、記述された"wrangler dev”コマンドが実行される

# cloudfareにデプロイする
npm run deploy
# 実際には、記述された"wrangler deploy"コマンドが実行される
```

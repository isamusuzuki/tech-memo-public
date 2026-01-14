# Node.jsの基本

作成日 2026/01/08、更新日 2026/01/14

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

## npm runコマンドについて

"scripts"設定に書かれたキーは、`npm run`コマンドで実行できる

例えば以下の通り

```bash
# 開発サーバーを起動する
npm run dev
# バリューに記述された"wrangler dev”コマンドが実行される

# cloudfareにデプロイする
npm run deploy
# バリューに記述された"wrangler deploy"コマンドが実行される

# startだけ特別で、run抜きで実行できる
npm start
# バリューに記述された"wrangler deploy"コマンドが実行される
```

## `npm run`コマンドが存在する理由

"scripts"項目のバリューに書かれたコマンド（ファイル）は、すべてnode_modulesの中にある

これはLinuxに限らないが、ファイル（コマンド）が実行されるためには、そのファイルがあるディレクトリが、PATH環境変数に記述されている必要がある。OSは、PATH環境変数に記述されているディレクトリを探し、ファイルが見つからなければ、エラーを返す

プロジェクト（リポジトリ）ごとに、node_modulesは存在するので、PATH環境変数に書くようなことはしない

`npm run`コマンドは、OSの代わりに、node_modulesの中にあるファイル（コマンド）を実行する

## `npx`コマンドとは何か

`npx`コマンドは、`npm run`コマンドと同じように、node_modulesの中にあるファイル（コマンド）を実行する。そして、package.jsonの"scripts"項目に書く必要がない

`npm run`コマンドには、長いコマンド文をあらかじめ登録しておき、短くタイプで実行できるという利便性があり、`npx`コマンドには、登録する手間がないという利便性がある

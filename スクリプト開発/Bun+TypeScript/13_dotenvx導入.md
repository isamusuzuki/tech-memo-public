# dotenvx導入

作成日 2025/05/28

## 1. dotenvxとは

- dotenvは、`.env`ファイルの中身を環境変数に組み込むツール
- dotenvxは、従来の`dotenv`のシンプルな使い勝手はそのままに、セキュリティとチーム開発のための機能を大幅に強化したツール

紹介記事 => [【脱・.env管理】dotenvxで環境変数を安全かつスマートに管理しよう！](https://qiita.com/channnnsm/items/2acea2a5ba54b30b28b4)

公式サイト（英語） => [dotenvx | a better dotenv–from the creator of dotenv](https://dotenvx.com/)

## 2. dotenvxのインストール方法

dotenvと同様に、npm installコマンドでNode.jsプロジェクトに組み込むことができる

```bash
npm install --save-dev @dotenvx/dotenvx
```

システム全体にインストールすると、`dotenvx`コマンドが使えるようになって、どの言語・どのプロジェクトでも使えるようになる

```bash
curl -sfS https://dotenvx.com/install.sh | sh
```

## 3. dotenvxを試す

```text
--sample-project/
    |--dist/
    |   `--index.js
    |--src/
    |   `--index.ts
    |--.env
    `--.gitignore  //.envを追加するのを忘れずに！
```

.env

```bash
NAME="World"
```

src/index.ts

```javascript
console.log(`Hello, ${process.env.NAME || 'Unknown'}!`);
```

コード実行

```bash
# 事前にTypeScriptをコンパイルする
npx tsc

npx dotenvx run -- node dist/index.js
# => # [dotenvx@1.44.1] injecting env (1) from .env
# => Hello, World!
```

## 4. dotenvxの暗号化機能

.envファイルを暗号化する

```bash
dotenvx encrypt
# => .env.valutファイルが生成される。リポジトリにコミットする
# => .env.keysファイルが生成される。リポジトリにコミットしない
```

復号化：`.env.keys`ファイルに記載されている`DOTENV_KEY`を環境変数に組み込む

```bash
# 開発環境のキーを設定して実行
export DOTENV_KEY="dotenv://:..."
dotenvx run -- node index.js

# または、コマンドの前に直接指定
DOTENV_KEY="dotenv://..." dotenvx run -- node index.js
```

CI/CD環境や本番環境では、`DOTENV_KEY`をシステムの環境変数に設定する

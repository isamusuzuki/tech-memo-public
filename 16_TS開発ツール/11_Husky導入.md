# Husky導入

Gitフックを利用してコード整形・自動テストを欠かさず実行するツール

作成日 2025/09/17、更新日 2025/09/18

## 1. 公式サイト（英語）を読む

[Husky](https://typicode.github.io/husky/)

```bash
# インストール
npm install --save-dev husky

# 雛形作成
npx husky init
# .husky/フォルダにpre-commitファイルを生成する
# package.jsonのscripts項目のprepareを更新（なければ生成）

# 全員実行
npm run prepare
# .husky/_/フォルダを生成する
```

## 2. 解説記事を読む

[Husky 入門：Node.js プロジェクトに Git Hooks を導入して、コミット品質を自動で守る](https://qiita.com/oharu121/items/2a38db1c76376de63cdb)

> prepareスクリプト: npm install時に自動でhusky installを実行
> チームメンバーがnpm installするだけでHuskyが有効になる

## 3. トラブル発生：フックが走らない

node_modules/husky/index.js

```javascript
// 11行目
if (!f.existsSync('.git')) return `.git can't be found`
```

`.git`フォルダが置かれたところにhuskyを組み込めば、`npx husky`コマンドで`.husky/_/`フォルダが生成される

モノリポのプロジェクトでhuskyを動かす方法はないのか？

## 4. [Project Not in Git Root Directory](https://typicode.github.io/husky/how-to.html#project-not-in-git-root-directory)

```text
--root/
    |--.git/
    |--backend/  # No package.json
    `--frontend/ # package.json with husky
```

`npx husky init`は叩かずに自分で準備する

package.json

```json
"scripts": {
  "prepare": "cd .. && husky frontend/.husky",
  "test": "eslint" // pre-commitで実行される 
}
```

frontend/.husky/pre-commit

```bash
cd frontend
npm test
```

`npm run prepare`を実行する => `frontend/.husky/_`が誕生する

コミットする度 => `npx eslint`が動作する

# TypeScriptを直接実行する

作成日 2021/05/12

ts-node を使うと、Typescriptのコードのまま、実行できる

インストール => `npm i -D ts-node`

ファイルの配置を変更する

```text
--avocado/
    |--src/
    |   `--main.ts  ... ここにあったものを
    |--main.ts      ... 直接ここに置く
    `--(main.js)    ... コンパイルしていたものは削除
```

```bash
# 今まで実行していたコードを
node main.js
# ts-nodeを使ったものに変更する
ts-node main.ts
```

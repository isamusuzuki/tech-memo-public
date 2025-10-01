# npmコマンド

作成日 2025/09/29

## 1. 公式ドキュメント（英語）を読む

[CLI Commands](https://docs.npmjs.com/cli/v11/commands)

- npm ci ... クリーンインストールの意味で、package.jsonを変更しない
- npm init ... package.jsonを生成する
- npm install ... パッケージをインストールする
- npm run ... scripts項目に書かれたコマンドを実行する
- npm test ... scripts項目のtestコマンドを実行する
- npx ... npmパッケージのコマンドを実行する

## 2. npmコマンドを複数フォルダで同時に実行する

解説記事 => [サブディレクトリでもnpmコマンドを複数同時に実行したい](https://qiita.com/algas/items/83c8a1df7ecf03177527)

> `npm install --prefix (SUB_DIRECTORY)` のように `--prefix` オプションでサブディレクトリを指定することで、サブディレクトリの `package.json` で定義されている、またはデフォルトで使える npm コマンドを実行することができます。

## 3. npmパッケージがどこから依存されているかを調べるコマンド

`npm install`を実行したときに、このパッケージは引退したと表示されることがある。そのパッケージはどこから依存されているのかを調べたいときに使うコマンド

```bash
npm explain inflight@1.0.6
```

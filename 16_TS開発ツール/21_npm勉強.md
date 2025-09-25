# npm勉強

作成日 2025/09/17、更新日 2025/09/25

## 1. [CLI Commands](https://docs.npmjs.com/cli/v11/commands)

- npm ci ... クリーンインストールの意味で、package.jsonを変更しない
- npm init ... package.jsonを生成する
- npm install ... パッケージをインストールする
- npm run ... scripts項目に書かれたコマンドを実行する
- npm test ... scripts項目のtestコマンドを実行する
- npx ... npmパッケージのコマンドを実行する

## 2. [scripts](https://docs.npmjs.com/cli/v11/using-npm/scripts)

特定の状況で発生するライフサイクルスクリプトがある

- prepare ... ローカルの引数なしの`npm install`の後に走る

### 2a. [Life Cycle Operation Order](https://docs.npmjs.com/cli/v11/using-npm/scripts#life-cycle-operation-order)

preinstallとprepareのコマンドをスクリプト項目に書き`npm ci`を実行したら、そのフォルダのnode_modulesへのインストールが先に起こって、そのあとに、preinstall, prepareの順番でスクリプトが実行された

> These all run after the actual installation of modules into `node_modules`, in order, with no internal actions happening in between

これらはnode_modulesへのモジュールの実際のインストールの後に走ると書いてあったので、それが正解なのだと思う。ここに書かれているinstallは、npm installのことではなく、installというコマンド名なのだと理解した

## 3. npmコマンドを複数フォルダで同時に実行する

解説記事 => [サブディレクトリでもnpmコマンドを複数同時に実行したい](https://qiita.com/algas/items/83c8a1df7ecf03177527)

> `npm install --prefix (SUB_DIRECTORY)` のように `--prefix` オプションでサブディレクトリを指定することで、サブディレクトリの `package.json` で定義されている、またはデフォルトで使える npm コマンドを実行することができます。

## 4. npmパッケージがどこから依存されているかを調べる

`npm install`を実行したときに、このパッケージは引退したと表示されることがある。そのパッケージはどこから依存されているのかを調べたいときに使うコマンド

```bash
npm explain inflight@1.0.6
```

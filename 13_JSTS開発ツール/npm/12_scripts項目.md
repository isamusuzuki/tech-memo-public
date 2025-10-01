# package.jsonのscripts項目

作成日 2025/09/29\

## 1. 公式ドキュメント（英語）を読む

[scripts | npm Docs](https://docs.npmjs.com/cli/v11/using-npm/scripts)

特定の状況で発生するライフサイクルスクリプトがある

- prepare ... ローカルの引数なしの`npm install`の後に走る

## 2. ライフサイクルスクリプトの実行順

[Life Cycle Operation Order](https://docs.npmjs.com/cli/v11/using-npm/scripts#life-cycle-operation-order)

preinstallとprepareのコマンドをscripts項目に書き`npm ci`を実行したら、\
そのフォルダのnode_modulesへのインストールが先に起こって、\
そのあとに、preinstall, prepareの順番でスクリプトが実行された

> These all run after the actual installation of modules into `node_modules`, in order, with no internal actions happening in between

これらはnode_modulesへのモジュールの実際のインストールの後に走ると書いてあったので、それが正解なのだと思う。ここに書かれているinstallは、npm installのことではなく、installというコマンド名なのだと理解した

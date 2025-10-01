# package.jsonのworkspaces項目

作成日 2025/09/29

## 1. 公式ドキュメント（英語）を読む

[workspaces | npm Docs](https://docs.npmjs.com/cli/v11/using-npm/workspaces)

Workspaces は、npm CLI の一連の機能の総称です。これを利用することで、単一のトップレベル、つまりルートパッケージ内から、ローカルファイルシステムにある複数のパッケージを管理できるようになります。

## 2. 解説記事を読む

[npm workspacesとモノレポ探検記](https://zenn.dev/suin/scraps/20896e54419069)

巻き上げ（hoisting） ... Node.jsのモジュール解決で、自分のnode_modulesフォルダに目的のモジュールが見つからないときは、親フォルダのnode_modulesフォルダ、その親フォルダのnode_modulesフォルダへとフォルダをさかのぼってモジュールを探す仕組みのこと

packages/aとpackages/bどちらもjestを使いたいとき、それぞれにjestをインストールするのではなく、ルートパッケージにjestをひとつだけ入れておけばいい。

```bash
# ワークスペース（サブフォルダ）を新規作成する
npm init -w {ワークスペース名}
# => サブフォルダが作成され、そこにpackage.jsonが生成される
# => ルートのpackage.jsonのworkspaces項目にサブフォルダ名が追加される

npm install
# => ルートのnode_modulesフォルダにワークスペースへのシンボリックリンクが生成される
# => 巻き上げによって、各ワークスペースから別のワークスペースを参照できるようになる

# ワークスペースにパッケージをインストールする
npm install {パッケージ名} -w {ワークスペース名}
# => ルートのnode_modulesフォルダにパッケージが追加される
# => サブフォルダのpackage.jsonのdependencies項目にパッケージ名が追加される
# => サブフォルダのpackage-lock.jsonも更新される

# 複数のワークスペースにパッケージをインストールする
npm install {パッケージ名} -w {ワークスペース名a}　-w {ワークスペース名b}

# すべてのワークスペースに一括してパッケージをインストールする
npm install {パッケージ名} -ws
# -ws は、--workspacesのアリアスと思われる

# ワークスペースをワークスペースにインストールする
npm install {ワークスペース名a} -w {ワークスペース名b}
```

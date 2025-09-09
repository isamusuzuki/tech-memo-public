# Node.jsインストール

作成日 2025/04/24、更新日 2025/07/02

## 1. Voltaコマンドとは

Node.jsのバージョン管理を行うコマンドラインツール

公式サイト（英語） => [Volta - The Hassle-Free JavaScript Tool Manager](https://volta.sh/)

参照サイト（日本語） => [Windows で Node.js のバージョン管理](https://note.com/rurai/n/n47a3fb9c4508)

wingetコマンドを使ってインストールできる

## 2. Voltaを使ってNode.jsをインストールする

```bash
volta install node@21.1.0
volta install npm@10.2.1
```

## 3. インストール済みのバージョンを一覧表示する

```bash
volta list node
volta list npm
```

## 4. プロジェクトで使用するNode.jsのバージョンを固定する

公式ガイド => [Managing your project](https://docs.volta.sh/guide/understanding#managing-your-project)

プロジェクトで`volta pin`コマンドを実行する

```bash
cd target-project
volta pin node@22.14.0
volta pin npm@10.9.2
```

package.jsonに書き込まれる

```json
{
  "volta": {
    "node": "22.14.0",
    "npm": "10.9.2"
  }
}
```

プロジェクトに携わる人で、かつVoltaを使っている人は、全員が同じバージョンを使うこととなる

```bash
npm install
```

## 5. 特定のバージョンのNode.jsをアンインストールする

voltaコマンドでアンインストールすることはできない

```bash
volta uninstall node@21.1.0
# error: Uninstalling node is not supported yet.
```

エクスプローラーを開く

`{HOME}\AppData\Local\Volta\tools\image\node\`

バージョン番号のディレクトリを手動で削除する

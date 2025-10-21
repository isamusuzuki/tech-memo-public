# npmスクリプトビュー

作成日 2025/10/21

## 1. npmスクリプトビューを開く

- `Ctrl + Shift + P`で、コマンドパレットを開く（Windowsの場合）
- `Explorer: Focus on NPM スクリプト View`コマンドを選択する

## 2. 右クリックしてデバッグを開始するとなぜかyarnコマンドが実行される

- `Ctrl + ,`で、設定を開く
- 検索窓に`npm`と入力して、左枠 ＞ 拡張機能 ＞ Npmを選択
- `Npm: Script Runner`を`auto`から`npm`に変更する

settings.json

```json
{
    "npm.scriptRunner": "npm"     // <= auto
}
```

`auto`設定は、ロックファイルに基づきコマンドを推測する。ワークスペースフォルダからpackage-lock.jsonを消して、親フォルダのpackage-lock.jsonに統合させると、`auto`設定は、yarnコマンドを実行してしまう

ちなみに、yarn (Classic)のロックファイルはyarn.lockだったが、Yarn (Plug'n'Play)は、node_modulesフォルダもyarn.lockファイルもない

# Windows ターミナル

作成日 2020/12/04、更新日 2021/06/12

## 01. Windows ターミナルとは

Microsoft ストアで配布されている新しいターミナルアプリ

ひとつのウィンドウの中に、複数のタブを用意して、コマンド プロンプト、PowerShell、WSL などを同時に実行可能

公式リポジトリ => [microsoft/terminal: The new Windows Terminal and the original Windows console host, all in the same place\!](https://github.com/microsoft/terminal)

## 02. Windows ターミナルを使うメリット

- Windows 環境でお馴染みの `Ctrl+C`, `Ctrl+V` でコピペができるところ

## Windows ターミナルのデフォルトを WSL に変更する

- タブの右にある「＋」アイコンのさらに右にある「下向き三角形」アイコンをクリックする
- ドロップダウンメニューの中から「設定」をクリックする
- 左枠 ＞ スタートアップ ＞ 右枠 ＞ 既定のプロファイル
- ドロップダウンリストで Ubuntu を選択する

## WSL を起動したときに開くディレクトリを変更する

デフォルトは Windows のホームフォルダになってしまっている。これを WSL 側のホームディレクトリに変更する

- 左枠 ＞ 「JSON ファイルを開く」をクリック
- json ファイルを開こうとする。デフォルトアプリを VSCode に設定する
- VSC の中で、`settings.json` ファイルを編集する
- `profiles.list` の中にある Wsl の 設定に `startingDirectory` を追加する

```json
{
  "profiles": {
    "list": [
      {
        "guid": "{abcdefg-hijklmn-opqrstu-vwxyz}",
        "hidden": false,
        "name": "Ubuntu",
        "source": "Windows.Terminal.Wsl",
        "startingDirectory": "\\\\wsl$\\Ubuntu\\home\\USER-NAME"
      }
    ]
  }
}
```

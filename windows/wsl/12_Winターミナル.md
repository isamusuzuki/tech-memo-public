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

- 左枠 ＞ プロファイル ＞ Ubuntu
- 右枠 ＞ ディレクトリの開始
- 「参照...」は押さずに、直接テキストボックスに以下を書き込む
- `\\wsl\$Ubuntu\home\USER-NAME`
- 右下 ＞ 「保存」ボタンをクリック

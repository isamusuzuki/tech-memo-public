# Remote WSL

作成日 2020/11/21、更新日 2020/12/02

## Remote WSL とは

WSL (Windows Subsystem for Linux) 上のフォルダを Visual Studio Code で開いて作業できるようにする機能拡張

[Remote \- WSL \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)

## Remote WSL を使う

前提条件

- WSL のセットアップと、好きな Linux ディストリビューションのインストールが終了している

正しい操作方法

- `Ctrl + Shift + P` でコマンドパレットを開く
- "Remote-WSL: Open Folder in WSL..." を検索して実行する
- 開きたいフォルダを選ぶ
- VSCode 左下の緑コーナーをクリックすることでも、開きたいフォルダを選べる
- `Ctrl + バッククォート` で統合ターミナルを開くと WSL 側の Bash が登場する

もっと簡単な操作方法

- WSL のターミナルを開く
- `code FOLDER-NAME` コマンドを叩く

最近は、WSL のターミナルを開きっぱなしにして、code コマンドで、いくつも VSCode を開いている

注意点

- 拡張機能は WSL 側にもインストールする必要がある
- WSL 側へのインストールは、拡張機能ビューの各拡張機能にリンクが登場する
- そのリンクをクリックして、VSCode を再起動するだけ

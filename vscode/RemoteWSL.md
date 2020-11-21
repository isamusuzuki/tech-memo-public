# Remote WSL

作成日 2020/11/21

## Remote WSL とは

WSL (Windows Subsystem for Linux) 上のフォルダを Visual Studio Code で開いて作業できるようにする機能拡張

[Remote \- WSL \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)

## Remote WSL を使う

前提条件

- WSL2 が使用できるバージョンの Windows 10 を使っている (2004, 20H2)
- WSL2 のセットアップは終了している

操作方法

- `Ctrl + Shift + P` でコマンドパレットを開く
- "Remote-WSL: Open Folder in WSL..." を検索して実行する
- 開きたいフォルダを選ぶ
- VSCode 左下の緑コーナーをクリックすることでも、開きたいフォルダを選べる
- `Ctrl + バッククォート` で統合ターミナルを開くと WSL 側の Bash が登場する

注意点

- 拡張機能は WSL 側にもインストールする必要がある
- インストール作業そのものは、拡張機能ビューに登場する指示に従うだけ

# Remote WSL

作成日 2020/11/21、更新日 2021/01/11

## 01. Remote WSL とは

WSL (Windows Subsystem for Linux) 上のフォルダを Visual Studio Code で開いて作業できるようにする機能拡張

[Remote \- WSL \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)

## 02. Remote WSL を使う

### 02-1. 前提条件

- WSL のセットアップと、好きな Linux ディストリビューションのインストールが終了している

### 02-2. 操作方法

- `Ctrl + Shift + P` でコマンドパレットを開く
- "Remote-WSL: Open Folder in WSL..." を実行する
- 開きたいフォルダを選ぶ
- VSCode 左下の緑コーナーをクリックすることでも、開きたいフォルダを選べる
- `Ctrl + バッククォート` で統合ターミナルを開くと WSL 側の Bash が登場する

### 02-3. もっと簡単な操作方法

- WSL のターミナルを開く
- `code FOLDER-NAME` コマンドを叩く

最近は、WSL のターミナルを開きっぱなしにして、code コマンドで、いくつも VSCode を開いている

### 02-4. Remote WSL の注意点

- Remote WSL 以外の拡張機能は WSL 側にインストールする必要がある
- WSL 側へのインストールは、拡張機能ビューの各拡張機能にリンクが登場する
- そのリンクをクリックして、VSCode を再起動する

### 02-5. Remote WSL の高度な使い方

- `Ctrl + Shift + P` でコマンドパレットを開く
- "Remote-WSL: Show Log" を実行する
- ログを読んでいろいろな情報を知る

## 04. Remote WSL トラブルシューティング

### 04-1. Web サーバーに Windows 側から localhost で接続できなくなった

Windows 側で設定が必要

`~/.wslconfig` というファイルを作成し、以下を書き込む

```bash
[wsl2]
localhostForwarding=true
```

今まで、この設定なしに出来ていたのはラッキーだっただけ。VirtualBox をインストールし、WSL 以外にも仮想イーサネットアダプターが登場した現在は、明示的な設定が必要になったと思われる

# WSL をインストールする

作成日 2021/11/15

## 01. 公式ガイドに従う

[WSL のインストール \| Microsoft Docs](https://docs.microsoft.com/ja-jp/windows/wsl/install)

### 前提条件

- スタートメニューの検索ボックスに "winver" と入力して、登場したコマンドを実行する
- 「Windows のバージョン情報」ウィンドウが登場する
- Windows のバージョンを確認する

### インストール

- "Windows PowerShell" を管理者として実行する
- 以下のコマンドを実行する

```powershell
wsl --install
```

## 02. WSL 管理コマンド

```powershell
# WSL 2 を既定のバージョンとして設定する
wsl --set-default-version 2

# インストール可能なディストリビューションの一覧を表示する
wsl --list --online

# Ubuntuをインストールする
wsl --install -d Ubuntu

# インストール済みのディストリビューションの一覧を表示する
wsl --list --verbose

# もしインストールしたディストリビューションがWSL2でなければ変更する
wsl --set-version Ubuntu 2
```

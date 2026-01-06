# WSLをインストールする

作成日 2026/01/06

## 1. 公式ドキュメントを読む

[WSL のインストール | Microsoft Learn](https://learn.microsoft.com/ja-jp/windows/wsl/install)

管理者モードでPowerShellを開き、以下のコマンドを入力して、再起動する

```bash
wsl --install
```

## 2. PowerShell上のWSLコマンド集

```bash
# バージョン番号を表示する
wsl --version

# 最新バージョンをインストールする
wsl --update

# インストール可能なディストリビューションの一覧を表示する
wsl --list --online

# インストール済みのディストリビューションの一覧を表示する
wsl --list -v

# Debianをインストールする
wsl --install -d Debian

# Debianを起動する
wsl -d Debian

# Debianを終了する
wsl --terminate Debian
```

### 初めてDebianを起動したとき

- セットアップに時間がかかるので、気長に待つ
- ユーザー名とパスワードの入力が求められる
- Windowsユーザーとは違う名前でかまわない

### sudoするたびにパスワードの入力を求められるのが面倒なとき

```bash
sudo visudo
# => 最終行に以下を追加する
# => YOURNAME ALL=(ALL) NOPASSWD: ALL
```

### ディストリビューションの再インストール

```bash
# インストール済みのディストリビューションの一覧を表示する
wsl --list -v

# インストール済みのDebianを削除する
wsl --unregister Debian

# Debianをインストールする
wsl --install -d Debian

# Debianをデフォルトに設定する
wsl --setdefault Debian

# デフォルトのディストリビューションを起動する
wsl
```

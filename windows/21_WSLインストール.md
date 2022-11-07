# WSL をインストールする

作成日 2021/11/15、更新日 2022/11/07

## 01. インストール

- "Windows PowerShell" を管理者として実行する
- 以下のコマンドを実行する

```powershell
wsl --install
```

- 「Windowsの機能の有効化または無効化」を起動させる
- 「Linux用 Windows サブシステム」項目にチェックを入れる
- 「仮想マシンプラットフォーム」項目にチェックを入れる
- OKボタンをクリックして、再起動させる

### 初めての起動

- セットアップに時間がかかるので、気長に待つ
- ユーザー名とパスワードの入力が求められる
- Windows ユーザーとは違う名前でかまわない

sudo するたびにパスワードの入力を求められるのが面倒ならば、以下の処理を行う

```bash
sudo visudo
# => 最終行に以下を追加する
# => YOURNAME ALL=(ALL) NOPASSWD: ALL
```

## 02. WSL 管理コマンド

```powershell
# WSL の状態を示す
wsl --status

# WSL 2 を既定のバージョンとして設定する
wsl --set-default-version 2

# インストール可能なディストリビューションの一覧を表示する
wsl --list --online

# Ubuntuをインストールする
wsl --install -d Ubuntu

# インストール済みのディストリビューションの一覧を表示する
wsl --list -v

# もしインストールしたディストリビューションがWSL2でなければ変更する
wsl --set-version Ubuntu 2
```

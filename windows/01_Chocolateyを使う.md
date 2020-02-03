# Chocolatey を使う

作成日 2020/02/03

## 01. Chocolatey とは

Windows のパッケージ管理マネージャー\
フリーのアプリは、Chocolatey 経由でインストールすることができる

公式サイト => [https://chocolatey.org/](https://chocolatey.org/)

### Chocolatey をインストールする

-   Powershell を管理者として実行する
-   公式サイトの[Installation ページ](https://chocolatey.org/install)を開く
-   `Install with PowerShell.exe` コーナーにあるスクリプトをコピペする
-   正常にインストールが完了したら、いったん PowerShell を閉じてからもう一度開く
-   `choco`とタイプして、バージョン番号が返ってくればインストール成功

### Chocolatey を使いこなす

```bash
# インストール済みパッケージから、更新可能なものを探す
choco outdated

# インストールしたパッケージを全てアップデートする
cup all -y

# インストールしたパッケージを調べる
choco list -lo(--local-only)

# パッケージをインストールする
cinst -y <package-name>

# パッケージをアンインストールする
cuninst -y <package-name>
```

### Chocolatey 経由でインストールしたアプリ

1. git.install
1. imagemagick.tool
1. nodejs-lts
1. python
1. selenium-chrome-driver

Google Chrome と Microsoft Visual Studio Code は、\
自動で更新する機能を持っているので、Chocolatey を使わない

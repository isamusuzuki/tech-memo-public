# Chocolatey を使う

作成日 2020/02/10

## 01. Chocolatey とは

Windows のパッケージ管理マネージャー\
フリーのアプリは、Chocolatey 経由でインストールすることができる

公式サイト => [https://chocolatey.org/](https://chocolatey.org/)

## 02. Chocolatey をインストールする

- Powershell を「管理者として実行」する
- 公式サイトの[Installation ページ](https://chocolatey.org/install)を開く
- `Install with PowerShell.exe` コーナーにあるスクリプトをコピペする
- 正常にインストールが完了したら、いったん PowerShell を閉じてからもう一度開く
- `choco`とタイプして、バージョン番号が返ってくればインストール成功

## 03. Chocolatey を使いこなす

```bash
# インストール済みパッケージから、更新可能なものを探す
choco outdated

# インストールしたパッケージを全てアップデートする
choco update all -y

# インストールしたパッケージを調べる
choco list -lo (--local-only)

# パッケージをインストールする
choco install -y <package-name>

# パッケージをアンインストールする
choco uninstall -y <package-name>
```

## 03. Chocolatey 経由でインストールするアプリ

1. git.install
1. python
1. nodejs-lts
1. gimp
1. imagemagick.tool
1. selenium-chrome-driver

Google Chrome と Microsoft Visual Studio Code は、\
自動で更新する機能を持っているので、Chocolatey を使わない

## 04. バージョン番号が違う複数の Python をインストールする

普通にインストールすると最新版がインストールされる\
現時点では、それは`3.8.1`である

```bash
# 最新版のインストール
choco install python

# バージョンを指定し、かつ共存させるインストール
choco install python --version=3.7.6 --force -y
choco install python --version=3.6.8 --force -y

# 共存の確認
C:\Python38\python --version
# => Python 3.8.1
C:\Python37\python --version
# => Python 3.7.6
C:\Python36\python --version
# => Python 3.6.8
```

### どれがデフォルトになるかは、環境変数の設定で決まる

コントロールパネル ＞ 右上 表示方法: 小さいアイコン ＞ システム\
左枠 ＞ システムの詳細設定 ＞ 「システムのプロパティ」ウィンドウ\
「詳細設定」タブ ＞ 環境変数ボタン ＞ 下段 システム環境変数\
テーブルの中から`Path`を選択 ＞ 編集ボタン\
どれを一番上に配置するかでデフォルトが決まる

- C:\Python38\Scripts\
- C:\Python38\
- C:\Python37\Scripts\
- C:\Python37\
- C:\Python36\Scripts\
- C:\Python36\

### プロジェクトごとに違うバージョンを利用するには

venv を使って、プロジェクトごとに仮想環境を構築する\
詳細は、vscode フォルダの 「Python 環境構築」を見よ

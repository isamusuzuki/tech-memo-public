# Linux開発環境

作成日 2019/06/01、更新日 2019/11/27

## 01. Linux をセットアップする

- Chrome OS のバージョンは `74.0.3729.159 (Official Build) (64ビット)`
- 設定項目のトップに「Linux（ベータ版）」があった
- 「オンにする」を選ぶとセットアップが始まった。10 分以上かかった
- 再起動すると、メニューに「Linux アプリ ＞ ターミナル」ができていた

### はじめてターミナルを起動する

```bash
# ホームフォルダを確認する
cd ~
pwd
# => /home/YOUR-NAME

# ディストリビューションとバージョンを確認する
cat /etc/os-release
# => PRETTY_NAME="Debian GNU/Linux 9 (stretch)"
# => NAME="Debian GNU/Linux"
# => ...

# パッケージを更新する
sudo apt update
sudo apt upgrade
```

### ChromeOS上のLinuxの注意事項

- まだ、スピーカー、マイク、カメラ、USBデバイスはサポートされていない
- ファイルアプリにおける「マイファイル ＞ Linuxファイル」は、Linuxのホームフォルダのこと

## 02. Visual Studio Codeをセットアップする

- vscodeの公式サイトに行き、最新のdebファイル 64bit用をダウンロード
- `code_1.34.0-1557957934_amd64.deb`を入手した
- ファイルアプリを使って、Linuxターミナル側にコピー
- 以下のコマンドを実行（ファイル名は、最後までタイプしなくても、途中でタブキーを押せば推測してくれる）

```bash
cd ~
sudo apt install ./code_1.34.0-1557957934_amd64.deb
```

- 終了したら、メニューにvscodeのアイコンができていた。ここから起動
- vscodeの新規ファイルに、日本語をコピペすることはできたが、入力することができない

## 03. 日本語入力システムのセットアップ

```bash
sudo apt install -y fcitx-mozc
fcitx -v
# => fcitx version: 4.2.9.1
```

- メニューにfcitxのアイコンができていた。ここから起動
- くるくる回ったままだが、次の作業に移る
- `fcitx-configtool`とタイプして、設定ツールを起動する
- 左下のプラスボタンをクリック ＞ 追加設定のダイアログ登場
- 検索のテキストボックスに`mozc`と入力 ＞ Only Show Current Pageのチェックボックスを外す ＞ Mozcが登場する
- Mozcを選択してから、OKボタンをクリック

### fcitxの起動を設定する

```bash
# ファイルを編集する
vi ~/.profile

# 最終行に一行追加する
fcitx > /dev/null >2&1
```

### vscodeで日本語が入力できることを確認する

- ターミナルを起動してから、vscodeを立ち上げる
- `Ctrl + Space`で日本語変換が起動する

## 04. Git を使えるようにする

### 開発リポジトリにSSH鍵を登録する

```bash
ssh-keygen
# 鍵を保存するファイル名はデフォルトのまま
# パスフレーズは指定しない（即エンターキー）

less id_rsa.pub
# 中身をコピーする
# GitLabにログインして、Settings > SSH Keys
# 鍵を貼り付けてから、Add keyボタン
```

### SSH 接続できるようにする

```bash
vi ~/.ssh/config
```

configファイル

```bash
Host gitlab.com
  Hostname gitlab.com
  User git
  IdentityFile ~/.ssh/id_rsa
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes
```

接続テスト

```bash
ssh git@gitlab.com
# => PTY allocation request failed on channel 0
# => Welcome to GitLab, @<name>!
# => Connection to gitlab.com closed.
```

あとは`git clone`してくるだけ

リポジトリにコミットするときの名前とメルアドの登録を忘れがちなので、要注意

```bash
git config --global user.name "Taro Okamoto"
git config --global user.email taro@example.com
```

## 05. Python開発環境

### Pythonはインストールされていた

```bash
python3 --version
# => Python 3.5.3

# pipとvenvのインストールが必要
sudo apt install -y python3-pip python3-venv
```

### venvで仮想環境を構築する

```bash
mkdir YOUR-PROJECT
cd YOUR-PROJECT

python3 -m venv .

# 仮想環境を有効化する
source bin/activate

# 仮想環境下では、pythonコマンドが使える
python --version
# => Python 3.5.3

# 仮想環境を無効にする
deactivate
```

`requirements.txt`ファイル

```text
autopep8
flake8
flake8-import-order
```

`.vscode/settings.json`ファイル

```json
{
  "python.pythonPath": "/home/YOUR-NAME/YOUR-PROJECT/bin/python",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.flake8Enabled": true,
  "python.linting.flake8Path": "/home/YOUR-NAME/YOUR-PROJECT/bin/flake8",
  "python.linting.lintOnSave": true,
  "python.formatting.provider": "autopep8",
  "python.formatting.autopep8Path": "/home/YOUR-NAME/YOUR-PROJECTqw/bin/autopep8",
  "editor.formatOnSave": true
}
```

## 06. ターミナルを終了するスクリプトを作成する

`~/.profile`ファイルに1行追加した

```bash
fcitx > /dev/null 2>&1
```

このfcitxをキルしないと、ターミナルを終了できない。

### `~./exit.sh` を作成する

```bash
pkill -e fcitx
pkill -e fcitx-dbus-watc
pkill -e mozc_server
```

pkillはプロセスIDではなく、プログラム名でキルできる

### 実行方法

```bash
cd ~
./exit.sh && exit
```

### 【補足】なぜ`~/.profile` ファイルだったのか？

ログインシェルは、`~/.bashrc` を読まない\
ログインシェルの読む順番は、`~/.bash_profile`, `~/.bash_login`, `~/.profile`

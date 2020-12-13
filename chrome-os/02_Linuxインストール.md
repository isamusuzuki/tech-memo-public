# Chrome OS に Linux をインストールする

作成日 2020/03/31

## 01. Linux をセットアップする

Chrome ブラウザで、`chrome://flags/#crostini-use-buster-image`を開き、Default から Enabled に変更する

左枠 ＞ Linux（ベータ版）＞ Linux コーナー ＞ 「オンにする」ボタンをクリック ＞ インストールが始まる

インストールが終わると、ターミナルが登場する

```bash
cat /etc/os-release
# => PRETTY_NAME="Debian GNU/Linux 10 (buster)"

sudo apt update
sudo apt upgrade -y

python3 --version
# Python 3.7.3
```

## 02. Visual Studio Code をセットアップする

- [vscode の公式サイト](https://code.visualstudio.com/)に行き、最新の deb ファイル 64bit 用をダウンロードする
- `code_1.43.0-1583783132_amd64.deb` (59.3MB)を入手した
- ファイルアプリを使って、Linux 側にコピーする
- 以下のコマンドを実行する（ファイル名は最後までタイプしなくても、タイプの途中でタブキーを押せば、推測してくれる）

```bash
cd ~
sudo apt install ./code_1.43.0-1583783132_amd64.deb
```

ランチャーに Visual Studio Code のアイコンができていた。これを起動する

日本語を表示することもできないし、ましてや日本語を入力することもできない

## 03. 日本語環境をセットアップする

### 日本語フォント

```bash
sudo apt install fonts-noto-cjk
```

Google Noto フォントをインストールしてから、vscode を再起動したら、日本語をコピペしても文字化けしなくなった

### かな漢字変換

```bash
sudo apt install fcitx-mozc

fcitx -v
# => fcitx version: 4.2.9.6
```

- ランチャーに fcitx のアイコンができている。これを起動する。くるくる回ったままの状態で次の作業へ
- `fcitx-configtool`とタイプして、設定ツールを起動する
- 左下のプラスをクリック、ダイアログ登場
- 検索のテキストボックスに`mozc`と入力
- Only Show Current Language のチェックボックスを外す
- Mozc が登場するので選択し、リストの一番上に移動させる

いつの間にか、fcitx のくるくるは止まっており、vscode で日本語が入力できるようになった

### fcitx を自動で起動させる

```bash
vi ~/.profile

# 最終行に一行追加する
fcitx > /dev/null 2>&1
```

反対に、fcitx が起動しているとターミナルを閉じることができないので、`~/exit.sh`を作成する

```bash
pkill -e fcitx
pkill -e fcitx-dbus-watcher
pkill -e mozc_server
```

このシェルスクリプトを実行できるようにする

```bash
chmod 755 exit.sh
```

ターミナルを閉じるときは以下のコマンドを叩く

```bash
./exit.sh && exit
```

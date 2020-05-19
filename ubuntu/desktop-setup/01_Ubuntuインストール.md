# Ubuntu 20.04 LTS をインストールする

作成日 2020/05/02、更新日 2020/05/16

## 01. ブートディスクを用意する

### ISO ファイルをダウンロードする

本家サイトから 2.5GB の ISO ファイルをダウンロードするのは時間がかかりすぎる\
IIJ のコピーサイトからダウンロードする

[Thank you for downloading Ubuntu Desktop \| Ubuntu](https://ubuntu.com/download/desktop/thank-you?version=20.04&architecture=amd64)

[Index of /pub/linux/ubuntu/releases](http://ftp.iij.ad.jp/pub/linux/ubuntu/releases/)

`ubuntu-20.04-desktop-amd64.iso` を入手したら、念の為にベリファイする\
やり方は、本家サイトにあった

```bash
echo "e5b72e9cfe20988991c9cd87bde43c0b691e3b67b01f76d23f8150615883ce11 *ubuntu-20.04-desktop-amd64.iso" | shasum -a 256 --check
```

[How to verify your Ubuntu download \| Ubuntu](https://ubuntu.com/tutorials/tutorial-how-to-verify-ubuntu)

### USB メモリに書き込む

balenaEtcher を使ったらなぜか失敗した\
本家チュートリアルは Rufus を使えとあったので、それに従う

[Create a bootable USB stick on Windows \| Ubuntu](https://ubuntu.com/tutorials/tutorial-create-a-usb-stick-on-windows)

[Rufus \- The Official Website \(Download, New Releases\)](https://rufus.ie/)

## 02. Ubuntu Desktop をインストールする

[Install Ubuntu desktop \| Ubuntu](https://ubuntu.com/tutorials/tutorial-install-ubuntu-desktop#1-overview)

- Welcome: English, Install Ubuntu ボタン
- Keyboard Layout: English (US) > English (US)
- Update and Other Software: Normal, Download updates
- Installation type: Erase disk and install Ubuntu
- Where are you?: Tokyo
- Who are you?: see below

| Key                           | Value    |
| ----------------------------- | -------- |
| Your name                     | taro     |
| Your computer's name          | avocado  |
| Pick a username               | taro     |
| Choose a password             | PASSWORD |
| Confirm your password         | PASSWORD |
| Require my password to log in | check    |

## 03. カスタマイズする

### 日本語入力を可能にする

fcitx と mozc をインストールする

```bash
sudo apt install fcitx-mozc -y
sudo apt purge ibus -y
```

- Input Method を起動する ＞ fcitx を選択する
- fcitx を起動する
- fcitx configuration tool を起動する ＞ リストに Mozc を追加する

再起動後、ターミナルを起動すれば、Mozc が登場するようになる

### CapsLock キーを Ctrl にキーに変更する

```bash
sudo vi /etc/default/keyboard
# XKBOPTIONS="" -> XKBOPTIONS="ctrl:nocaps"
```

設定を反映させるために再起動する

### Chromium をインストールする

apt コマンドではなく、Ubuntu Software を使う

### Visual Studio Code をインストールする

[Visual Studio Code \- Code Editing\. Redefined](https://code.visualstudio.com/)

deb パッケージをダウンロードする ＞ code_1.44.2-1587079832_amd64.deb (62.3MB)

```bash
cd ~/Downloads
sudo apt install ./code_1.44.2-1587079832_amd64.deb
```

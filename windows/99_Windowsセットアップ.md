# Windows 10 バージョン 2004 をセットアップする

作成日 2020/07/11、更新日 2020/07/18

## 01. ISO ファイルのダウンロード

[Windows 10 のディスク イメージ \(ISO ファイル\) のダウンロード](https://www.microsoft.com/ja-jp/software-download/windows10ISO)

選択順 Windows ＞ 64-bit ＞ 日本語

`Win10_2004_Japanese_x64.iso` (4.8GB) をダウンロードする

## 02. Ubuntu で USB メモリへ書き込む

woeUSB アプリをインストールし、これを使う

[How to Create a Bootable Windows 10 USB on Ubuntu](https://www.omgubuntu.co.uk/2017/06/create-bootable-windows-10-usb-ubuntu)

このアプリは libwxgtk3.0 が必要

[Ubuntu – Package Download Selection \-\- libwxgtk3\.0\-0v5_3\.0\.4\+dfsg\-3_amd64\.deb](https://packages.ubuntu.com/bionic/amd64/libwxgtk3.0-0v5/download)

どちらもずっと使い続けるつもりはないので、deb ファイルをダウンロードして、\
ローカルにあるファイルを apt コマンドでインストールする\
目的を達成したらアンインストール

```bash
cd ~/Downloads
sudo apt install ./libwxgtk3.0-0v5_3.0.4+dfsg-3_amd64.deb
sudo apt install ./woeusb_3.3.1-1_webupd8_focal0_amd64.deb

sudo apt purge woeusb
sudo apt purge libwxgtk3.0-0v5
```

## 03. Windows 10 をセットアップする

指示された通りに進めるだけなので、省略

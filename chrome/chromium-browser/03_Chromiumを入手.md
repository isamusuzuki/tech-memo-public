# Chromium を入手する

作成日 2021/05/23

## Ubuntu 版の Chromium を入手するには Snap を使う

```bash
sudo apt install snapd

sudo snap install chromium
```

しかし、WSL2 上の Ubuntu では snapd は動作していない

Playwright が Chromium をダウンロードするその元を調べてみた

## Playwright のコードから該当する箇所を発見

[https://github.com/microsoft/playwright/blob/master/utils/check_chromium_cdn.js](https://github.com/microsoft/playwright/blob/master/utils/check_chromium_cdn.js)

- `https://storage.googleapis.com/chromium-browser-snapshots/Linux_x64/LAST_CHANGE` => ここを叩くと、最新のバージョン番号が返ってくる。今ならば `885823`
- `https://storage.googleapis.com/chromium-browser-snapshots/Linux_x64/{{LATEST_VERSION}}/chrome-linux.zip` => ここを叩くと、ZIPファイルをダウンロードできる

## Chromium の公式サイトが紹介しているダウンロード方法も全く同じだった

[Download Chromium \- The Chromium Projects](https://www.chromium.org/getting-involved/download-chromium)

> Easy Point and Click for latest build: Open up [https://download-chromium.appspot.com](https://download-chromium.appspot.com)

## Chromium をインストールする

ダウンロードしたzipファイルを解凍して、WSL2 上の Ubuntu のホームフォルダに置く

```bash
cd /mnt/c/Users/<USER>/Downloads
unzip chrome-linux.zip -d ~
ln -s /home/<USER>/chrome-linux/chrome /home/<USER>/chrome
./chorme &
```

# Ubuntu でテレカンできるようにする

作成日 2020/08/05

## 01. 概要

業務で使っている Viber と Zoom を Ubuntu でできるようにして、\
業務から iPad を外す

## 02. 手持ちの機材の互換性を確認する

- [x] ソニーワイヤレスヘッドホンを接続して、音声が聞けるか => OK
- [x] ミニマイクを PC フロントに接続して、自分の声を録れるか => OK
- [x] 有線イヤホンを PC フロントに接続して、音声が聞けるか => OK

真夏に両耳を塞ぐヘッドホンは蒸れるので、有線イヤホンに切り替えた

### 音声設定についてわかったこと

PC フロントの 2 つのジャック穴は、向かって左がヘッドホン、右がマイク

Settings ＞ 左枠 ＞ Sound ＞ 右枠 ＞ Output\
Output Device を "Headphones - Starship/Matisse HD Audio Controller" に変更する\
Input Device を "Front Microphone - Starship/Matisse HD Audio Controller" に変更する

## 03. 新たにアプリをインストールする

### Ubuntu Software を使う

- [x] Audacity ... 録音の確認用
- [x] viber-unofficial ... Viber 非公式アプリ、IME を有効にできない

### Zoom アプリをインストールする

公式サイトから、deb パッケージと公開鍵をダウンロードする

[Download Center \- Zoom](https://zoom.us/download?zcid=1231)

```bash
cd ~/Downloads
ls
# => zoom_amd64.deb

sudo apt install ./zoom_amd64.deb
```

## 04. Ubuntu 互換の Web カムを探す

参考記事 => [10 Best Webcams for Ubuntu in 2020 – Linux Hint](https://linuxhint.com/best_webcams_ubuntu_2020/)

Ubuntu で動くためには、その機器が、USB Video Device Class (UVC) mode で動作可能なことが必要

- Pro Stream Webcam
- Logitech HD Pro Webcam C920
- Angetube Streaming Webcam 1536P
- Razer Kiyo
- Logitech C922x Pro Stream Webcam
- ... 以下省略 ...

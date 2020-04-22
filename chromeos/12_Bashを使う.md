# Debian 上の Bash を使う

作成日 2020/04/15

## 01. 便利なショートカットキー

- `Ctrl + U` ... 入力中の文字を削除する
- `Ctrl + l` ... Clear コマンドを実行する

## 02. .bashrc に環境変数を追加する

Debian では、ちょっと書き方が違った

```bash
TWILIO_ACCOUNT_SID="ABCDEFG"; export TWILIO_ACCOUNT_SID
TWILIO_AUTH_TOKEN="1234567"; export TWILIO_AUTH_TOKEN
```

## 03. apt update コマンドで NO_PUBKEY エラー発生

```text
The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 78BD65473CB3BD13
```

公開鍵が入手できないというメッセージのようだ

### 解決策

公開鍵を鍵サーバーから入手する

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 78BD65473CB3BD13
```

## 04. ImageMagick を使う

```bash
# imagemagickをインストールする
sudo apt install imagemagick -y

convert --version
# => Version: ImageMagick 6.9.7-4 Q16 x86_64 20170114 http://www.imagemagick.org

# ファイルアプリで、Linuxファイルの中に新しいフォルダをつくり
# その中に、変換したい画像ファイルをコピーする
cd images

# 画像ファイルを一括でサイズ変換する
# 横幅のサイズだけを指定。縦横の比率は保たれる
mogrify -resize 200 *.jpg
```

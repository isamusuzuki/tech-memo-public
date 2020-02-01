# Debian を使いこなす

作成日 2020/02/02

## 01. `.bashrc`に環境変数を追加する

Debian では、ちょっと書き方が違った

```bash
TWILIO_ACCOUNT_SID="ABCDEFG"; export TWILIO_ACCOUNT_SID
TWILIO_AUTH_TOKEN="1234567"; export TWILIO_AUTH_TOKEN
```

## 02. 画像ファイルを一括でサイズ変換する

ImageMagick を使う

```bash
# imagemagickをインストールする
sudo apt install imagemagick -y

convert --version
# => Version: ImageMagick 6.9.7-4 Q16 x86_64 20170114 http://www.imagemagick.org

# ファイルアプリで、Linuxファイルの中に新しいフォルダをつくり
# その中に、変換したい画像ファイルをコピーする
cd images

# 横幅のサイズだけを指定。縦横の比率は保たれる
mogrify -resize 200 *.jpg
```

## 03. Docker を動かす

[Get Docker Engine \- Community for Debian \| Docker Documentation](https://docs.docker.com/install/linux/docker-ce/debian/)

```bash
# 必要となるパッケージをインストール
sudo apt install apt-transport-https ca-certificates \
  curl gnupg2 software-poroperties-common

# apt認証キーの追加
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88

# aptリポジトリの追加
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"

# aptリポジトリの更新
sudo apt update

# dockerパッケージのインストール
sudo apt install docker-ce docker-ce-cli containd.io

# 一般ユーザーで使えるようにするための設定
sudo groupadd docker
sudo usermod -aG docker $USER
```

コンソールからログアウトした後、再びログイン

```bash
# dockerのバージョン確認
docker --version

# nginxコンテナの起動
docker run -d -p 8080:80 --name webserver nginx

# コンテナの動作確認
docker ps -a
```

## 04. `apt update`コマンドで NO_PUBKEY エラーが発生した

```text
The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 78BD65473CB3BD13
```

公開鍵が入手できないというメッセージのようだ

### 解決策

公開鍵を鍵サーバーから入手する

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 78BD65473CB3BD13
```

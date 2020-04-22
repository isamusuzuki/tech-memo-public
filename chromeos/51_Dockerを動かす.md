# Docker を動かす

作成日 2020/02/02

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

---
tags: ubuntu
---

# Dockerをインストールする

作成日 2019/12/05

公式ドキュメント => [https://docs.docker.com/install/linux/docker-ce/ubuntu/](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

```bash=
# 必須パッケージをインストール
sudo apt install apt-transport-http \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
 
# Dockerの公式GPGキーをインストール
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 
# 鍵指紋の確認
sudo apt-key fingerprint 0EBFCD88
# => 指紋の最後の10個を確認する
# => 9DC8 5822 9FC7 DD38 854A
# => E2D8 8D81 803C 0EBF CD88

# リポジトリの追加
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# やっとインストール
sudo apt update
sudo apt-get install docker-ce docker-ce-cli containerd.io

# バージョンの確認
apt-cache medison docker-ce

# テストドライブ
sudo docker run hello-world


# 通常ユーザーでもdockerを利用できるようにする
sudo gpasswd -a $(whoami) docker
sudo chgrp docker /var/run/docker.sock
sudo service docker restart
```


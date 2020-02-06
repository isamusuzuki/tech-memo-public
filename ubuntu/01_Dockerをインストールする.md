# Dockerをインストールする

作成日 2019/12/12

## 01. 公式ドキュメントを写経する 

[Get Docker Engine \- Community for Ubuntu \| Docker Documentation](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

### Install using the repository

SET UP THE REPOSITORY

```bash
# Update tha apt package index
sudo apt update

# install packages to allow apt to use a repository over HTTPS
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify that you now have the key with the fingerprint
sudo apt-key fingerprint 0EBFCD88
# => uid [ unknown] Docker Release (CE deb) <docker@docker.com>

# Set up the stable repository
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

INSTALL DOCKER ENGINE - COMMUNITY

```bash
# Update tha apt package index
sudo apt update

# Install the latest version of Docker Engine - Community and containerd
sudo apt install docker-ce docker-ce-cli containerd.io
```

### 自分の場合はインストール中にエラーが起こった

「dockerを起動できない」というメッセージだったので、ひとまず再起動した

```bash
# デーモンを確認する
systemctl status docker
systemctl status containerd

# Verify that Docker Engine
sudo docker run hello-world
# => Hello from Docker!
# => This message shows that your installation appears to be working correctly.
```

=> 問題なし

### Manage Docker as a non-root user

[Post\-installation steps for Linux \| Docker Documentation](https://docs.docker.com/install/linux/linux-postinstall/)

sudoなしでdockerコマンドを実行できるようにする

```bash
sudo groupadd docker
# => group 'docker' already exists

sudo usermod -aG docker $USER
```

いったんログアウトする必要あり

```bash
# sudoなしでdockerコマンドが実行できることを確認する
docker run hello-world
# => Hello from Docker!
# => This message shows that your installation appears to be working correctly.
```

### Install Docker Compose

Ubuntuの場合は、Dockerに含まれておらず、別途インストールする必要がある

[Install Docker Compose \| Docker Documentation](https://docs.docker.com/compose/install/)

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
# => docker-compose version 1.25.0, build 0a186604
```

最新のバージョン番号はここを見る => [Releases · docker/compose](https://github.com/docker/compose/releases)

# Ubuntu 20.04 に Docker をインストールする

作成日 2021/06/13、更新日 2022/01/03

## 01. Install Docker Engine on Ubuntu

[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

### Set up the repository

```bash
sudo apt update

sudo apt install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
"deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# これは以下と同じ
cd /etc/apt/sources.list.d
sudo touch docker.list
sudo vi docker.list
# deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu focal stable

# このコマンドはUbuntuのリリース名を返す
lsb_release -cs
# => focal
```

### Install Docker Engine

```bash
sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io

# 正しくインストールされたことを確認する
sudo docker run hello-world
```

## 02. Post-installation steps for Linux

[https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/)

### Manage Docker as a non-root user

```bash
sudo groupadd docker
# => groupadd: group 'docker' already exists

sudo usermod -aG docker $USER
# いったんログアウトして、もう一度ログインし直す

docker run hello-world
```

## 03. Docker Compose をインストールする

Linux の場合は、Docker Compose のバイナリを別途インストールする必要がある

Install Docker Compose\
[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# これは以下と同じ
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-Linux-x86_64" -o /usr/local/bin/docker-compose

uname -s
# => Linux

uname -m
# => x86_64

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
# => docker-compose version 1.29.2, build 5becea4c
```

※ `1.29.2` は、1系の最終バージョンである模様。2021年9月に `2.0.0` がリリースされて、2系のバージョンだけが更新されている

[https://github.com/docker/compose/tags](https://github.com/docker/compose/tags)

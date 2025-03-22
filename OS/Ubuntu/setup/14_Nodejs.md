# Ubuntu に Node.js をインストールする

作成日 2021/01/17、更新日 2022/03/25

## 01. 最新の LTS のバージョンを調べる

公式サイト => [https://nodejs.org/en/](https://nodejs.org/en/)

最新の LTS バージョンは `14.15.4` だった

`apt install` でインストールできるバージョンを確認する

```bash
apt search nodejs
# => nodejs/focal 10.19.0~dfsg-3ubuntu1 amd64
```

普通に `apt install nodejs` をしただけでは、最新の LTS のバージョンはインストールされないことを確認した

## 02. 最新の LTS をインストールするために Nodesource を使う

Nodesource は、Linux 用の Node.js のバイナリーを配布しているところ

公式サイト => [https://github.com/nodesource/distributions](https://github.com/nodesource/distributions)

```bash
curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash -

sudo apt install nodejs -y

node -v
# => v14.15.4

npm -v
# => 6.14.11

npx -v
# => 6.14.11

npm list -g --depth=0
# => /usr/lib
# => └──npm@6.14.11
```

## 03. Node.js をアンインストールする

[How to remove nodejs from nodesource.com? - Ask Ubuntu](https://askubuntu.com/questions/629315/how-to-remove-nodejs-from-nodesource-com)

```bash
sudo apt purge nodejs

cd /etc/apt/sources.list.d
sudo rm nodesource.list

sudo reboot
```

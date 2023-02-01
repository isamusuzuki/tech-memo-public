# `apt upgrade` でエラーが発生

作成日 2021/07/27、更新日 2021/07/29

## 2021/07/27 10:30 問題発生

```bash
apt search ubuntu-advantage-tools
# => ubuntu-advantage-tools/focal-updates,now 27.1~20.04.1 amd64 [installed,automatic]
# => management tools for Ubuntu Advantage

sudo apt update

apt list --upgradable
# => ubuntu-advantage-tools 27.2.1~20.04.1 があった

sudo apt upgrade -y
# => Setting up ubuntu-advantage-tools (27.2.1~20.04.1) ...
# => Installing new version of config file /etc/apt/apt.conf.d/20apt-esm-hook.conf ...
# => Installing new version of config file /etc/ubuntu-advantage/uaclient.conf ...
# => ERROR: File not found '/run/cloud-init/instance-data.json'. Provide a path to instance data json file using --instance-data
# => dpkg: error processing package ubuntu-advantage-tools (--configure):
# =>  installed ubuntu-advantage-tools package post-installation script subprocess returned error exit status 1

sudo apt update
# => All packages are up to date.

sudo apt upgrade -y
# => 同じエラー
```

## 対処

パッケージをアンインストールしてしまう

```bash
sudo apt remove ubuntu-advantage-tools -y
sudo apt autoremove -y
```

## 2021/07/29 問題解決

パッケージのバージョンが新しくなって、問題が解消されていた

27.2.1 => 27.2.2

```bash
sudo apt update
apt search ubuntu-advantage-tools
# => ubuntu-advantage-tools/focal-updates 27.2.2~20.04.1 amd64 [upgradable from: 27.2.1~20.04.1]

sudo apt install ubuntu-advantage-tools -y
```

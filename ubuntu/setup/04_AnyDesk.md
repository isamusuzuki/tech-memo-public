# AnyDesk

作成日 2020/06/14

## AnyDesk とは

個人利用は無料のリモートデスクトップ接続アプリ\
Windows や Mac だけでなく、Linux や Raspberry Pi までサポートしている

[高速リモートデスクトップアプリケーション – AnyDesk](https://anydesk.com/ja)

## AnyDesk をインストールする

[Remote desktop sharing with AnyDesk on Ubuntu 20\.04 Focal Fossa \- LinuxConfig\.org](https://linuxconfig.org/remote-desktop-sharing-with-anydesk-on-ubuntu-20-04-focal-fossa)

```bash
wget -qO - https://keys.anydesk.com/repos/DEB-GPG-KEY | sudo apt-key add -
sudo sh -c 'echo "deb http://deb.anydesk.com/ all main" > /etc/apt/sources.list.d/anydesk-stable.list'
sudo apt update
sudo apt install anydesk -y
```

## AnyDesk を設定する

### リモート接続される側

AnyDesk アプリを起動する\
This Desk コーナー ＞ Set password for unattended access...\
Settings 画面登場 ＞ 左枠 ＞ Security ＞ Unlock Seurity Settings...\
Unattended Access コーナー ＞ 「Enable unattended access」にチェック入れる\
Set password for unattended access...

### リモート接続する側

AnyDesk アプリを起動する\
Remote Desk コーナー ＞ 相手側の AnyDesk-Address を入力する\
相手側のパスワードを入力する

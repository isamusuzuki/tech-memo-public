# AnyDesk

作成日 2020/06/14、更新日 2020/11/22

## 01. AnyDesk とは

個人利用は無料のリモートデスクトップ接続アプリ。Windows や Mac だけでなく、Linux や Raspberry Pi までサポートしている

公式サイト => [https://anydesk.com/ja](https://anydesk.com/ja)

## 02. AnyDesk をインストールする

[https://linuxconfig.org/remote-desktop-sharing-with-anydesk-on-ubuntu-20-04-focal-fossa](https://linuxconfig.org/remote-desktop-sharing-with-anydesk-on-ubuntu-20-04-focal-fossa)

```bash
wget -qO - https://keys.anydesk.com/repos/DEB-GPG-KEY | sudo apt-key add -
sudo sh -c 'echo "deb http://deb.anydesk.com/ all main" > /etc/apt/sources.list.d/anydesk-stable.list'
sudo apt update
sudo apt install anydesk -y
```

## 03. AnyDesk を設定する

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

## 04. AnyDesk トラブルシューティング

### リモートから再起動したら接続できなくなった

`Not Supported: Remote display server is not supported (e.g. Wayland)` というメッセージが出る

### 解決方法

/etc/gdm3/custom.conf

```text
[daemon]
# Uncomment the line below to force the login screen to use Xorg
WaylandEnable=false

# Uncomment the line below to force the login screen to use Xorg
AutomaticLoginEnable = true
AutomaticLogin = $USERNAME
```

### Wayland とは何か

参考記事 => [Ubuntu 21\.04 その 12 \- Ubuntu on Wayland をデフォルトにする提案 \- kledgeb](https://kledgeb.blogspot.com/2021/01/ubuntu-2104-12-ubuntu-on-wayland.html)

> 「Wayland」は現在の「X」を置き換える仕組みであり、将来的に「X」から「Wayland」へ移行することになります。
>
> 「Wayland」自身はディスプレイサーバーとそのクライアント間の通信方法を定めた仕様であり、その仕様に基づいて実装されたソフトウェアが複数存在しています。
>
> 現在「Ubuntu」では、従来の「X」上で動作する「Xorg セッション」と、「Wayland」上で動作する「Wayland セッション」を提供しています。どちらも利用可能ですが、デフォルトでは「Xorg」セッションが選択されています。

### 理解したことを検証する

確かにログイン画面で選択できるセッションのデフォルトが入れ替わっていた

Ubuntu 20.04 のログイン画面

- Ubuntu ... Xorg セッション
- Ubuntu on Wayland ... Wayland セッション

Ubuntu 21.04 のログイン画面

- Ubuntu ... Wayland セッション
- Ubuntu on Xorg ... Xorg セッション

### AnyDesk は、Wayland セッションをサポートしていない

公式説明 1 => [AnyDesk on Linux \- AnyDesk Help Center](https://support.anydesk.com/AnyDesk_on_Linux)

> **Important:** Please keep in mind that Wayland sessions (selectable in your login screen) aren´t supported. Please make sure an Xorg session is running.

公式説明 2 => [Installation \- AnyDesk Help Center](https://support.anydesk.com/Installation)

> **Note:** please keep in mind that on Wayland sessions, selectable in login screen, only outgoing sessions are supported. Incoming sessions are possible only within Xorg session.

### どちらのセッションかを調べる方法

```bash
echo $XDG_SESSION_TYPE
# => x11 or wayland
```

# Windows 10 に WSL をインストールする

作成日 2019/08/19

## 導入手順

コントロールパネル ＞ プログラムと機能 ＞ Windows の有効化または無効化\
Windows Subsystem for Linux にチェックを入れる ＞ OK ボタン ＞ 「今すぐ再起動」ボタン\
Microsoft Store で Ubuntu を入手する\
Ubuntu のダウンロードが終わったら起動する\
初回の起動は時間がかかる\
ユーザー名とパスワードは、Windows と同じものを設定する\
ひとつの PC に、複数のアカウントを設定している場合は、それぞれ Microsoft Store から入手する必要がある\
それはログインするユーザー名とパスワードを設定するためだと思う

```text
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username: user
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
Installation successful!
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

user@PC4059653: $
```

## インストールされた OS を確認する

```bash
cat /etc/os-release
#=> VERSION="18.04.3 LTS (Bionic Beaver)"

arch
#=> x86_64
```

## Ubuntu を最新状態にする

```bash
sudo apt update
sudo apt upgrade -y
```

## 日本語環境を整える

```bash
locale
#=> LANG=en_US.UTF8

locale -a
#=> ja_JP.utf8が一覧にない

sudo apt install -y language-pack-ja

locale -a
#=> ja_JP.utf8 が追加されている

sudo update-locale LANG=ja_JP.UTF8
# いったんログアウトする

locale
#=> LANG=ja_JP.UTF8
```

## Python 開発環境を整える

```bash
python --version
#=> Command 'python' not found
python3 --version
#=> Python 3.6.8
pip3 --version
#=> Command 'pip3' not found

sudo apt install -y python3-pip python3-venv
```

## Node.js 開発環境を整える

```bash
node -v
#=> Command 'node' not found

curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt install -y nodejs

node -v
#=> v10.16.3

npm -v
#=> 6.9.0
```

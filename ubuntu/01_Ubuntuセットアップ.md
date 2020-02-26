# Ubuntu をセットアップする

作成日 2020/02/03、更新日 2020/02/18

## 01. Ubuntu のインストーラーを調達する

Ubuntu のホームページに行く => [https://ubuntu.com/](https://ubuntu.com/)\
ダウンロードコーナーで、Ubuntu Desktop ＞ 18.04 LTS をクリック\
`ubuntu-18.04.3-desktop-amd64.iso` (1.93GB) を入手\
[balenaEtcher](https://www.balena.io/etcher/)で USB メモリに書き込む

## 02. Ubuntu をインストールする

### ウィザード画面

| 画面名                     | 実行したこと                                     |
| -------------------------- | ------------------------------------------------ |
| Welcome                    | English ＞ Continue ボタン                       |
| Keyboard layout            | English(US), English(US) ＞ Continue ボタン      |
| Updates and other software | Minimal installation ＞ Continue ボタン          |
| Installation type          | Erase disk and install Ubuntu ＞ Continue ボタン |
| Where are you?             | Tokyo ＞ Continue ボタン                         |
| Who are you?               | 下の表の通り ＞ Continue ボタン                  |

### Who are you? の項目

| ラベル                        | 入力値        |
| ----------------------------- | ------------- |
| Your name                     | ubuntu        |
| Your computer's name          | HOST-NAME     |
| Pick a username               | ubuntu        |
| Choose a password             | YOUR-PASSWORD |
| Confirm your password         | YOUR-PASSWORD |
| Log in automatically          | check on      |
| Require my password to log in | check off     |

## 03. パッケージを更新する

```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
```

## 04. タイムゾーンを変更する

```bash
date
# => Tue Feb 18 08:43:59 UTC 2020

sudo timedatectl set-timezone Asia/Tokyo

date
# => Tue Feb 18 17:44:59 JST 2020
```

## 05. 別 PC から SSH 接続できるようにする

```bash
# IPアドレスを調べようと思ったら、ifconfigがなかった
# 今後使うツールはこれ
ip address

# sshdをインストールする
sudo apt install aptitude
sudo aptitude install ssh
systemctl status ssh
```

## 06. GitHub リポジトリに接続できるようにする

公開鍵ファイル・秘密鍵ファイルを作成する

```bash
cd ~

# RSA暗号の鍵を生成する
ssh-keygen -t rsa
# => ファイル名もパスフレーズも指定しない
# => /home/ubuntu/.ssh/id_rsa, id_rsa.pub ファイルが生成される
```

別 PC から scp コマンドを実行して、公開鍵ファイルを取得する

```bash
scp ubuntu@192.168.2.174:/home/ubuntu/.ssh/id_rsa.pub ~/
```

別 PC で、公開鍵を GitHub に登録する

Ubuntu に戻って、`~/.ssh/config` 設定ファイルを作成する

```text
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes
```

GitHub に ssh 接続を試みて、設定が間違っていないことを確認する

```bash
ssh git@github.com
# => ssh接続は許可されていないので終了するが、
# => 設定は間違っていないことがわかる
```

Git クローンする

```bash
git clone git@github.com:your-name/repository-name.git
```

# SSH 接続される側の管理

作成日 2021/03/24

## 01. 別 PC から SSH 接続できるようにする

```bash
# IPアドレスを調べる
ip address

# sshdをインストールする
sudo apt install aptitude
sudo aptitude install ssh
systemctl status ssh
```

## 02. authorized_keys ファイル

自分が SSH 接続される場合に、相手側の公開鍵をこのファイルに書き込んでおくと、公開鍵でのログインが可能になる

公開鍵を登録した後は、ssh の設定を変更して、パスワード認証を使えなくする

```bash
# 公開鍵を登録する
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

# パスワード認証を使えなくする
sudo vi /etc/ssh/sshd_config
# => PasswordAuthentication yesをnoに変更する

# 再起動が必要
sudo shutdown -r now
```

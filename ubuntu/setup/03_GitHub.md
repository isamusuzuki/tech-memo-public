# GitHub に接続できるようにする

作成日 2020/03/12、更新日 2020/06/13

## Git をインストールする

```bash
sudo apt git install
```

## 自分の名前とメルアドを設定する

```bash
git config --global user.name "Taro Okamoto"
git config --global user.email taro@example.com
```

## 公開鍵ファイル・秘密鍵ファイルを作成する

```bash
cd ~

# RSA暗号の鍵を生成する
ssh-keygen -t rsa
# => ファイル名もパスフレーズも指定しない
# => /home/ubuntu/.ssh/id_rsa, id_rsa.pub ファイルが生成される
```

## GitHub に公開鍵を登録する

```bash
# 公開鍵を表示する
less .ssh/id_rsa.pub

# 鍵指紋を確認する
ssh-keygen -E md5 -l -f .ssh/id_rsa
```

## SSH 接続できるように設定する

`~/.ssh/config` 設定ファイルを作成する

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
# => 表示される英語をよく読むと、
# => 設定は間違っていないことがわかる
```

## GitHub からリポジトリをクローンする

```bash
git clone git@github.com:your-name/repository-name.git
```

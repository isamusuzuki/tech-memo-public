# Git 環境構築

作成日 2020/03/12

## GitHub リポジトリに接続できるようにする

公開鍵ファイル・秘密鍵ファイルを作成する

```bash
cd ~

# RSA暗号の鍵を生成する
ssh-keygen -t rsa
# => ファイル名もパスフレーズも指定しない
# => /home/ubuntu/.ssh/id_rsa, id_rsa.pub ファイルが生成される

# 鍵指紋を確認する
ssh-keygen -E md5 -l -f .ssh/id_rsa
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
# => 表示される英語をよく読むと、
# => 設定は間違っていないことがわかる
```

Git クローンする

```bash
git clone git@github.com:your-name/repository-name.git
```

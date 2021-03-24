# ssh コマンド

作成日 2019/09/28、更新日 2020/02/05

## 01. ssh コマンドの基本

構文: `scp オプション ログイン名@ホスト名`

オプション

- `-i` ... SSH 秘密鍵を指定する
- `-F` ... config ファイルを指定する

使用例

```bash
ssh -i ~/.ssh/awsweb.pem centos@10.0.0.224
ssh -F ./ssh_config bingo-server

# LAN上のラズパイに接続する
ssh pi@192.168.0.110
# => パスワードを聞かれるので、入力する

# 秘密鍵を使ってサーバーに接続する
ssh -i ~/.ssh/secret.pem ec2-user@100.100.100.100

# リモート側のスクリプトを実行する
ssh -i ~/.ssh/secret.pem ec2-user@100.100.100.100 ./something.sh
# => 実行が終わったら即切断される
```

## 02. config ファイル

- `~/.ssh`フォルダに、`pem`ファイルを置く
- `~/.ssh`フォルダに、`config`ファイルを作成し、以下のように編集する

```text
Host bingo-server
  Hostname 100.100.100.100
  User ubuntu
  IdentityFile ~/.ssh/aws-web.pem
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes
```

ssh 接続を確認

```bash
ssh bingo-server
ssh bingo-server -F ~/config
```

`-F`オプションを使えば、config ファイルはどこにでも置けるようになる

## 03. known_hosts ファイル

接続したことのある各ホストの公開鍵を保存している

ホストに ssh 接続することなく known_hosts レコードに登録する方法は、以下の通り

```bash
# 対象ホストの公開会議を取得し、ファイルに追記する
ssh-keyscan -H example.com >> ~/.ssh/known_hosts

# known_hostsから対象ホストの公開鍵を削除する
ssh-keygen -R example.com
```

## 04. SSH 鍵の鍵指紋を確認する

```bash
ssh-keygen -l -E md5 -f ~/.ssh/id_rsa
ssh-keygen -l -E md5 -f ~/.ssh/id_rsa.pub
```

- 公開鍵、秘密鍵のどちらを指定しても、鍵指紋は同じ結果になる
- `-E`オプションを使い、`MD5/hex`で表示させると、GitHub/GitLab で表示されているものと同じになる

# ssh コマンド

作成日 2019/09/28、更新日 2020/02/05

## 01. ssh コマンドの基本

構文: `scp オプション ログイン名@ホスト名`

オプション

-   `-i` ... SSH 秘密鍵を指定する
-   `-F` ... config ファイルを指定する

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

### 兄弟のような scp コマンド

構文: `scp オプション ログイン名@ホスト名:パス ログイン名@ホスト名:パス`

オプション

-   `-i` ... SSH 秘密鍵を指定する
-   `-r` ... ローカルのディレクトリごとリモートにコピーする

使用例

```bash
# 最も簡単な例
scp ~/test.txt ubuntu@192.168.3.251:~/test.txt

# SSH秘密鍵を使い、フォルダごとまとめてコピーする
scp -i ~/.ssh/awsweb.pem -r ./ centos@10.0.0.224:/opt/bitnami/apps/wordpress/htdocs/work/
```

## 02. config ファイル

-   `~/.ssh`フォルダに、`pem`ファイルを置く
-   `~/.ssh`フォルダに、`config`ファイルを作成し、以下のように編集する

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

## 04. authorized_keys ファイル

自分が、SSH 接続される場合に、相手側の公開鍵を、このファイルに書き込んでおくと、\
公開鍵でのログインが可能になる

公開鍵を登録した後、ssh の設定を変更して、パスワード認証を使えなくする方法は、以下の通り

```bash
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

sudo vi /etc/ssh/sshd_config
# => PasswordAuthentication yesをnoに変更する

# 再起動が必要
sudo shutdown -r now
```

## 05. SSH 鍵の鍵指紋を確認する

```bash
ssh-keygen -l -E md5 -f ~/.ssh/id_rsa
ssh-keygen -l -E md5 -f ~/.ssh/id_rsa.pub
```

-   公開鍵、秘密鍵のどちらを指定しても、鍵指紋は同じ結果になる
-   `-E`オプションを使い、`MD5/hex`で表示させると、GitHub/GitLab で表示されているものと同じになる

## 06. SSH Port Forwarding

紹介記事を読む

[Intel NUC で自宅 Ubuntu 開発環境構築と SSH Port Forwarding によるアクセス \| blog\.jxck\.io](https://blog.jxck.io/entries/2019-11-03/nuc-dev-server-port-forwarding.html)

> 自宅内に置いているため、固定 IP などはない。\
> しかし、せっかく作った環境は、外出先等でも使いたいため、外からもアクセスできるようにしたい。\
> すでに Sakura VPS には固定 IP を振っているため、これを用いた最も安価で簡単な方法は SSH の Portfowarding だろう。\
> NUC と VPS の SSH を張りっぱなしにしておき、laptop からの SSH をそこを通して NUC に届けるのが Port Fowarding だ。

```bash
# nuc から vps
ssh user@vps -NR 22222:localhost:22
# => 繋ぎっぱなしにする

# laptop から vps
ssh user@vps

# vps に入ったあと vps から nuc
ssh user@localhost -p 22222
```

## 07. w コマンド, tty コマンド

[自分がどの IP アドレスで SSH 接続しているのかを確認する ｜ Developers\.IO](https://dev.classmethod.jp/cloud/aws/amazon-linux-2-ssh-ipaddress/)

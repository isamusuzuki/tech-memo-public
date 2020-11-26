# Ubuntu 20.04 へ do-release-upgrade する

作成日 2020/11/07、更新日 2020/11/20

## 01. はじめての do-release-upgrade

Ubuntu 18.04.5 LTS が動作している AWS EC2 インスタンス にログインすると、以下の文言が表示されるようになった

```text
New release '20.04.1 LTS' available.
Run 'do-release-upgrade' to upgrade to it.
```

snapshot を取ってから、do-release-upgrade を実行してみることにした

### 実戦

```bash
sudo do-release-upgrade
```

上記のコマンドを実行したら、最初に SSH 接続であることを指摘された。1022 ポートにもうひとつ sshd デーモンを起動するか？ と聞いてきた

これにはイエスと答えないと先に進めない

所属しているセキュリティグループのインバウンドルールに、1022 ポートを追加し、もうひとつターミナルを表示して、以下のコマンドで SSH 接続して先に進んだ

```bash
ssh -p 1022 ubuntu@100.100.100.100 -i ~/.ssh/something.pem
```

結果は成功したが、実はそれはビギナーズラックであった。2 台目の更新から、トラブル続きとなった。この記事のメインはトラブルシューティングである

## 02. MySQL が消えた

### 事件発生 1

MySQL データベースを動かしているサーバーで do-release-upgrade したら、システムを再起動した後、MySQL がなくなっていた

do-release-upgrade の作業している最中に、「`/var/lib/mysql` を削除するか？」というとんでもない質問が登場した。もちろんノーと返事したが、つまりは、MySQL のプログラムがアンインストールされていたのだった

### 解決方法 1

MySQL が消えた理由は、Ubuntu 20.04 での MySQL のバージョンが 5.7 から 8.0 に上がったことだった。さっそく再インストールを行う

```bash
sudo apt install mysql-server
sudo mysql_secure_installation
# => 同じ root のパスワードを設定する

mysql -u root -p
# => ログインできた

mysql> SHOW databases;
# => データベースは残っていることが確認できた

mysql> USE mysql;
mysql> SELECT user, host FROM user;
mysql> \q

mysql -u user -D db -p
# => ログインユーザーも残っていることが確認できた
```

ただし、外部からはログインできなくなっていた。もう一度設定する必要があった

```bash
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
# => [mysqld]
# => bind-address = 127.0.0.1 をコメントアウトする

sudo systemctl restart mysql
```

=> これで一件落着

## 03. SSL 接続エラーが発生した事件

### 問題発生 2

Ubuntu 18.04 を動かしていた本番サーバーで、`do-release-upgrade` したら、FTPS 接続ができなくなった。詳しく見てみると、以下のエラーメッセージが出ていた

```text
[SSL: DH_KEY_TOO_SMALL] dh key too small (_ssl.c:1123)
```

関連記事を読むと、Ubuntu 20.04 になってセキュリティが厳しくなり、設定が古いサーバーへの接続を拒否するようになった事がわかった

#### 関連記事

[Ubuntu 20\.04 で SSL 接続エラー（ssl\.SSLError: \[SSL: DH_KEY_TOO_SMALL\]）が発生する際の対処法 \- Qiita](https://qiita.com/sidious/items/f9a6eaaf6b1786d6a92c)

[Ubuntu 20\.04 \- how to set lower SSL security level? \- Ask Ubuntu](https://askubuntu.com/questions/1233186/ubuntu-20-04-how-to-set-lower-ssl-security-level)

### 解決方法 2

OpenSSL の設定ファイルをいじって、セキュリティレベルを下げる

```bash
openssl version -d
# => OPENSSLDIR: "/usr/lib/ssl"

cd /usr/lib/ssl
ls -l
# => lrwxrwxrwx 1 root root   20 Apr 20  2020 openssl.cnf -> /etc/ssl/openssl.cnf

cd /etc/ssl
sudo cp openssl.cnf openssl.cnf.org

sudo vi openssl.cnf
```

`openssl.cnf` の冒頭に 1 行追加する

```text
openssl_conf = default_conf
```

`openssl.cnf` の末尾に以下を追加する

```text
[ default_conf ]

ssl_conf = ssl_sect

[ssl_sect]

system_default = system_default_sect

[system_default_sect]
MinProtocol = TLSv1.2
CipherString = DEFAULT:@SECLEVEL=1
```

=> サーバーを再起動したら、エラーはでなくなった

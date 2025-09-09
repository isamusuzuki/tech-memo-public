# sed コマンド

作成日 2020/02/03

## ファイルの中のある箇所を置換する

構文1: `sed -e "s/置換ルール/置換文字/g" 置換するファイル名`\
構文2: `cat 読み込むファイル名 | sed -e "s/置換ルール/置換文字/g" > 吐き出すファイル名`

オプション

- `-e` ... 次にくる処理命令を実行する
- `-i` ... 処理内容を同じファイルに書き込む
- `s` ... 正規表現で置換処理する
- `g` ... すべてのマッチした文字列を置換する

使用例

```bash
sed -i -e 's/jkl/JKL/g' ./test.txt
sed -i -e 's@/etc/foo/bar@/home/my/etc@g' ./test.txt
```

## スラッシュをエスケープするのが面倒なとき

違う記号を使えばいい。`\n`で改行も追加できる

使用例

```bash
cat /etc/sysconfig/network | sed -e "s|^NETWORKING.*|NETWORKING=yes|" -e "s|^HOSTNAME.*|HOSTNAME=lamp.vagrantup.com|" > /tmp/network
sudo cp -p /etc/sysconfig/network /etc/sysconfig/network.old
sudo cp -p /tmp/network /etc/sysconfig/network
rm -f /tmp/network

cat /etc/sysconfig/selinux | sed 's/^SELINUX=.*$/SELINUX=disabled/' > /tmp/selinux
sudo cp -p /etc/sysconfig/selinux /etc/sysconfig/selinux.old
sudo cp -p /tmp/selinux /etc/sysconfig/selinux
rm -f /tmp/selinux

sed -i -e 's@DocumentRoot /var/www/html@#DocumentRoot /var/www/html\n<Document /var/www/html>@g' ./test.txt
```

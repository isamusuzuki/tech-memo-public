# xauth 勉強

作成日 2021/03/24、更新日 2021/03/25

## 01. 問題発生

サーバー側の Chromium をローカルで動かしてみようと思った

### サーバー側に Chormium をインストールする

```bash
sudo snap install chromium
# => chromium 89.0.4389.90 from Canonical✓ installed

which chromium
# => /snap/bin/chromium
```

### 起動するとエラーが発生した

```bash
chromium &
# => X11 connection rejected because of wrong authentication.
```

参照記事 => [Forwarding X11 using ssh on modern desktop](http://vega.pgw.jp/~kabe/vsd/ssh-x.html)

> X11 connection rejected because of wrong authentication.
>
> remote 側で xauth add をやっていない。
> 対処：remote 側で手動で xauth add を発行する。

## 02. xauth とは

- xauth は、X アプリケーションを動かす際のサーバーとクライアント間の認証の仕組み
- X サーバー側で、クッキーを生成し、それをクライアント側に登録することで認証される
- `~/.Xauthority` ファイルは、登録されたクッキーが含まれているバイナリーファイル

```bash
# ~/.Xauthorityに保存されているマジッククッキーを表示する
xauth list
# => ip-172-31-26-41/unix:10  MIT-MAGIC-COOKIE-1  b2ffd23c92d1159b65c0d8371f17e218
```

WSL には、xauth コマンドがインストールされているが、`~/.Xauthority` ファイルはない。そもそも X サーバー は、WSL ではなくて、VcXsrv である

```bash
which xauth
# => /usr/bin/xauth

xauth list
# => xauth:  file /home/<user>/.Xauthority does not exist
```

Windows のコマンドプロンプトを起動する

```bash
cd C:\Program Files\VcXsrv
dir xauth.exe
# => xauth.exe が用意されている

xauth --help
# => usage:  xauth [-options ...] [command arg ...]

xauth add
xauth extract
xauth generate
xauth list
xauth merge
```

## 03. 参照記事を念入りに読む

[ubuntu \- WSL2 の中の X client から VcXsrv に xauth で接続したい \- スタック・オーバーフロー](https://ja.stackoverflow.com/questions/66736/wsl2%E3%81%AE%E4%B8%AD%E3%81%AEx-client%E3%81%8B%E3%82%89-vcxsrv-%E3%81%AB-xauth-%E3%81%A7%E6%8E%A5%E7%B6%9A%E3%81%97%E3%81%9F%E3%81%84)

Windows コマンドプロンプトで、マジッククッキーを生成する方法がわかった

ついでに、WSLの中ではWindows 側は、`コンピュータ名.mshome.net` であることもわかった

```bash
cd C:\Program Files\VcXsrv

# WSL側のIPアドレスを調べる
ipconfig
# => 172.29.192.1

xauth generate 172.29.192.1:0.0
# => xauth:  file \users\Isamu\.Xauthority does not exist

xauth list
# => DESKTOP-8PIKTF5.mshome.net:0  MIT-MAGIC-COOKIE-1  bac36a70122749b382e238acfeab08c9

xauth extract - DESKTOP-8PIKTF5.mshome.net:0.0
# => バイナリの呪文が登場した
# => "| xauth merge -" で引き継げば、X クライアントにコピーが完了する
```

WSL上でそれを実行するスクリプト

```bash
WXAUTH="/mnt/c/Program Files/VcXsrv/xauth.exe"
HN=$(hostname).mshome.net
( cd /mnt/c ; "$WXAUTH" generate $HN:0.0 . trusted timeout 0 )
( cd /mnt/c ; "$WXAUTH" extract - $HN:0.0 ) | ssh ubuntu@100.100.100.100 xauth merge -
```

[\(X Window System\) 昔、xhostって使っちゃいけないコマンドだと教わったのですが・・・xauthをよろしく \- ttt](https://blog.goo.ne.jp/nhh0/e/a070a9b0edde6a52c1ace2b351ced261)

[WSL2からアクセス制御\(Xauthority\)を有効にしたX Serverに接続する \- Qiita](https://qiita.com/YasuhiroABE/items/9926ead0cee8c6d65576)

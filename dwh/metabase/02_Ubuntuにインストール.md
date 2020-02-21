# Metabase を Ubuntu にインストールしてみる

作成日 2019/12/05

## 01. Jar ファイルを入手する

Docker を動かす方法もあるが、とりあえず Jar ファイルを動かしてみて、\
一刻も早く試してみることにした

公式ドキュメント => [Running the Metabase Jar File](https://www.metabase.com/docs/latest/operations-guide/running-the-metabase-jar-file.html)

Ubuntu 18.04 には、openjdk-8-jdk も、openjdk-11-jdk も、\
パッケージが用意されているが、ドキュメントに合わせることにした

```bash
cat /etc/os-release
# => NAME="Ubuntu"
# => VERSION="18.04.3 LTS (Bionic Beaver)"

sudo apt install openjdk-8-jdk -y

java -version
# => openjdk version "1.8.0_222"
```

最新版の jar ファイルのありか => [https://www.metabase.com/start/jar.html](https://www.metabase.com/start/jar.html)

ダウンロードボタンの行き先 URL をコピペする

```bash
wget http://downloads.metabase.com/v0.33.6/metabase.jar
```

## 02. 最初から管理データベースを MySQL にする

参考記事 => [metabase を mysql で動かす \- Qiita](https://qiita.com/kanaxx/items/bf3dd4ca2d9d09e73da4)

Metabase は、デフォルトでは、管理データを H2 データベースに保存する

H2 データベースとは、Java プラットフォーム上で動く、小さなデータベース

最初から MySQL を使うようにする

すでにインストール済みの MySQL に、Metabase 専用のデータベースとユーザーを作成する

```sql
CREATE DATABASE metabase DEFAULT CHARACTER SET utf8;
GRANT ALL PRIVILEGES ON metabase.* TO metauser@localhost IDENTIFIED BY 'metaPasswd1!';
```

起動用のシェルスクリプトを作成する

`~/metabase.sh`ファイル

```bash
set MB_DB_TYPE=mysql
set MB_DB_DBNAME=metabase
set MB_DB_PORT=3306
set MB_DB_USER=metauser
set MB_DB_PASS=metaPasswd1!
set MB_DB_HOST=localhost
java -jar metabase.jar
```

シェルスクリプトを実行する

```bash
cd ~
touch metabase.sh
vi metabase.sh

chmod 755 metabase.sh

./metabase.sh
```

ブラウザで`http://192.168.2.82:3000`を開いたら、セットアップの開始画面になった

![](https://imgur.com/Yza3yj1.png)

## 03. Metabase をデーモン化する

[ubuntu18 に Metabase を割としっかりインストールする](https://wwld.jp/2019/05/25/metabase.html)

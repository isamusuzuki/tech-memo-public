# Docker を使いこなす

作成日 2019/12/12

## 01. よく使う docker コマンド一覧

```bash
# イメージを引っ張ってくる
docker pull alpine

# タグを使ってバージョンを指定する
docker pull alpine:latest

# コンテナに名前をつけて起動して、シェルを開く
docker run -it --name shell1 alpine /bin/sh

# 終了するときにコンテナを破棄する
docker run -it -rm --name shell2 alpine /bin/sh

# コンテナにホストディレクトリをマウントする
docker run -it -v c:/tmp:/tmp alpine /bin/sh

# バックグランドで起動する
docker run -d -p 8080:80 --name web1 nginx

# 起動したコンテナの一覧を表示する
docker ps

# 起動していないコンテナも含めて一覧を表示する
docker ps -a

# コンテナを停止させる
docker stop <container>

# コンテナを強制停止させる
docker kill <container>

# コンテナを削除する
docker rm <container>

# 停止しているコンテナをすべて削除する
docker rm $(docker ps -aq)

# イメージの一覧を表示する
docker images

# イメージを削除する
docker rmi <image>

# 全イメージを削除する
docker rmi $(docker images -aq)

# コンテナ内のPID 1のプロセスに対して接続する
# コンテナ実行時に-itオプションしている必要がある
docker attach <container>

# コンテナ内に新しいプロセスを追加して実行する
# コンテナ実行時に-dオプションをしていても大丈夫
docker exec -it <container> bash
```

## 02. よく使う docker イメージ一覧

- [alpine](https://hub.docker.com/_/alpine) ... 最小の Linx 5.55MB 最新は 3.10.3
- [nginx](https://hub.docker.com/_/nginx) ... 最新は 1.17.6
- [mysql](https://hub.docker.com/_/mysql) ... 最新は 8.0.18
- [python](https://hub.docker.com/_/python) ... 最新は 3.8.0

## 03. Dockerfile とは

[Dockerfile リファレンス](http://docs.docker.jp/engine/reference/builder.html)

> Docker は Dockerfile から命令を読み込み、自動的にイメージを構築できます。\
> Dockerfile はテキスト形式のドキュメントであり、コマンドライン上でイメージを\
> 作り上げる命令を全て記述します。\
> ユーザは docker build を使い、複数のコマンド行の命令を順次実行し、\
> イメージを自動構築します。

### Dockerfile の EXPOSE 命令と docker コマンドの-p フラグは、どう使い分けるのか

- EXPOSE 命令はコンテナがそのポートをリッスンすることを Docker に伝える
- EXPOSE 命令だけあっても、これだけではホストからコンテナにアクセスできない
- ホストからコンテナにアクセスするには、`-p`フラグを使ってポート転送を設定する必要がある

### Dockerfile の VOLUME 命令と docker コマンドの-v フラグは、どう使い分けるのか

- VOLUME 命令は、マウントポイントを作成し、外部マウント可能なボリュームにする
- `-v`フラグはコンテナにデータボリュームを追加する
- `-v`フラグは、ホスト上にあるディレクトリもコンテナにマウント可能
- VOLUME 命令は、ホスト上にあるディレクトリをコンテナにマウントすることはできない

`-v`フラグの使い方

- `-v /webapp` ... webapp という新しいボリュームを作成する
- `-v /opt/webapp:ro` ... 読み込み専用でボリュームをマウントする
- `-v /src/webapp:/opt/webapp` ... ホスト側のディレクトリをコンテナにマウントする

### 自分が考える Dockerfile の役割

Dockerfile の役割は、Docker イメージの構築である\
構築されたイメージはポータブルでないといけない。どこでも使えないといけない\
Docker ホストに依存するホストディレクトリのマウントを、\
Dockerfile で指示することはできない\
必要なファイルはイメージの中にコピーするのが王道

## 04. docker-compose とは

`docker-compose.yml`ファイルと`docker-compose`コマンドを使って、\
複数のコンテナを操作する

ドキュメント => [Overview of Docker Compose \| Docker Documentation](https://docs.docker.com/compose/)

写経すべし => [Get started with Docker Compose \| Docker Documentation](https://docs.docker.com/compose/gettingstarted/)

### Compose ファイルの書き方を学ぶ

[Compose file version 3 reference \| Docker Documentation](https://docs.docker.com/compose/compose-file/)

Compose ファイルの基本

- `build:`オプション ... ローカルにある Dockerfile からイメージを作成する
- `images:`オプション ... Docker レジストリにあるイメージをそのまま使用する

docker-compose コマンド

```bash
docker-compose up -d nginx mysql redis workspace
docker-compose stop
docker-compose down
docker-compose rm
```

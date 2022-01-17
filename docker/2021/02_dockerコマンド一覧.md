# docker コマンド一覧

作成日 2021/06/13

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

# docker-compose とは

作成日 2021/06/13

`docker-compose.yml`ファイルと`docker-compose`コマンドを使って、複数のコンテナを操作する

ドキュメント => [Overview of Docker Compose \| Docker Documentation](https://docs.docker.com/compose/)

写経すべし => [Get started with Docker Compose \| Docker Documentation](https://docs.docker.com/compose/gettingstarted/)

## Compose ファイルの基本

[Compose file version 3 reference \| Docker Documentation](https://docs.docker.com/compose/compose-file/)

- `build:`オプション ... ローカルにある Dockerfile からイメージを作成する
- `images:`オプション ... Docker レジストリにあるイメージをそのまま使用する

## docker-compose コマンドの基本

```bash
docker-compose up -d nginx mysql redis workspace
docker-compose stop
docker-compose down
docker-compose rm
```

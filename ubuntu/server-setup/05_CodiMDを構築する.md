# 自前 HackMD サーバーを構築する

作成日 2019/11/08、更新日 2019/12/13

## 01. CodiMD とは

CodMD は、HackMD のオープンソース版

リポジトリ => [hackmdio/codimd: CodiMD \- Realtime collaborative markdown notes on all platforms\.](https://github.com/hackmdio/codimd)

ドキュメント => [CodiMD Documentation \- HackMD](https://hackmd.io/c/codimd-documentation/%2Fs%2Fcodimd-documentation)

### Docker Deployment をやってみる

[Docker Deployment \- HackMD](https://hackmd.io/c/codimd-documentation/%2Fs%2Fcodimd-docker-deployment)

`docker-compose.yml`ファイル

```yaml
version: '3'
services:
    database:
        image: postgres:11.5
        environment:
            - POSTGRES_USER=codimd
            - POSTGRES_PASSWORD=change_password
            - POSTGRES_DB=codimd
        volumes:
            - 'database-data:/var/lib/postgresql/data'
        restart: always
    codimd:
        image: nabo.codimd.dev/hackmdio/hackmd:1.4.0
        environment:
            - CMD_DB_URL=postgres://codimd:change_password@database/codimd
            - CMD_USECDN=false
        depends_on:
            - database
        ports:
            - '3000:3000'
        volumes:
            - upload-data:/home/hackmd/app/public/uploads
        restart: always
volumes:
    database-data: {}
    upload-data: {}
```

起動

```bash
docker-compose up -d
```

## 02. 解説記事を読む

[そこそこセキュアな自前 HackMD サーバー構築 \- Qiita](https://qiita.com/suecharo/items/14f4d0b5430db811254e)

> HackMD のココが駄目\
> 画像を貼ると imgur に Upload される\
> いわゆる URL を知っているすべての人が閲覧できます状態\
> ノートは private にできるが設定し忘れると上の状態になる
>
> docker を使う\
> Nginx を使う\
> Let's encrypt を使う

[CodiMD を社内オンプレ環境に構築する \- Qiita](https://qiita.com/atsuyuuki/items/072c41cb9405264cf368)

[HackMD \+ Nginx で社内ナレッジをイイ感じに整理する \- アトトックラボ｜株式会社アトトック](https://www.atotok.co.jp/labo/eeea24f937be3be52882bf535a4e3cf7)

> ユーザの認証方法なんかも指定できます。今回はメール認証で。\
> 他にも facebook, twitter, github, gitlab, dropbox, google... などなどが指定できます。お好みで。\
> defaultPermission 　では記事を書いたときの初期権限を設定できます。\
> 今回は社内ナレッジのため、LIMITED にしています。

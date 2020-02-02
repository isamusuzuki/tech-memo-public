# Metabase を学ぶ

作成日 2019/12/02、更新日 2019/12/05

## 01. Metabase とは

オープンソースのデータ可視化ツール

公式トップ => [https://www.metabase.com/](https://www.metabase.com/)

[Getting Started](https://www.metabase.com/docs/latest/getting-started.html)

> Metadata は、シンプルでパワフルな分析ツールです。誰でも覚えることができ、\
> 自分の会社のデータを使った意思決定ができるようになります。\
> 技術知識は不要です！\
> あなたがこれを愛することを願っています

## 02. Metabase の紹介記事

### [OSS のデータ可視化ツール「Metabase」が超使いやすい](https://qiita.com/acro5piano/items/0920550d297651b04387)

> インストールが簡単。openjdk-8-jdk 入れて、jar ファイル置くだけ。\
> 豊富なデータソースに対応。MySQL・PostgreSQL・GoogleAnalytics・BigQuery など。\
> 結果を直感的にいい感じに可視化してくれ、グラフ種類が豊富。
>
> Kibana は、ダッシュボードがめっちゃかっこいいんだけど、DB が KVS や Elasticsearch 前提\
> MySQL に入ってるデータを SQL で取ってきて見たいんだよなーみたいな時に、\
> いやまず RDB のスキーマを Elasticsearch のインデックスにマッピングして同期しないと。\
> Elasticsearch のクエリを覚えるのが、学習コスト高い。

### [成果の可視化でチームの心を 1 つにしよう！ツール選定と Metabase による実際の手順をご紹介！](https://service.plan-b.co.jp/blog/tech/16103/)

### [『Metabase』が滅茶苦茶使いやすくて感動した件](https://www.yutorism.jp/entry/Metabase)

### [Metabase 使って秒速で Nginx のアクセスログを可視化してみる](https://qiita.com/code_monkey/items/acc8b78430dffb323813)

> nginx のアクセスログを td-agent 使って mysql へ格納\
> metabase で可視化
>
> ダッシュボードの作成
>
> -   1 週間
> -   上段に日毎のアクセス
> -   中段にユーザエージェント・IP アドレス・ステータスコード
> -   下段に生データ

### [Docker 上の Metabase で Pandas の DataFrame をお手軽に眺める](https://qiita.com/nekodango/items/2538c1a314b7ec5cb379)

> 最近人気急上昇の Metabase と Pandas を組み合わせる方法を共有します。\
> iris データを読み込ませて、自動探査(X-ray)してみました。
>
> この Metabase、使いやすさと見た目のかっこよさはダントツなのですが、データソースはデータベースからの読み込みのみ(ブラウザ上で「手元の csv データをアップロードしてインポートする」ができない)な模様です。そこで、Docker 上に Metabase およびデータベース(PostgreSQL)を立て、そのデータベースに対して Dataframe の中身を書き込みます。

### [Metabase を Docker 上で構築して Redshift へ接続する](https://dev.classmethod.jp/business/bigdata/metabase_ond_cors/)

```text
--avocado/
    |--docker-compose.yml
    |--env
    |--metabase-data
    |--postgres-data
    |--PG_VERSION
    |--base
    |    |--1
    ....
    `--postmaster.pid
```

`docker-compose.yml`ファイル

```yaml
version: '2'
services:
    metabase:
        image: metabase/metabase
        env_file: ./env
        volumes:
            - ./metabase-data:/metabase-data
        ports:
            - 3000:3000
        depends_on:
            - postgres-mb

    postgres-mb:
        image: postgres:9.5-alpine
        env_file: ./env
        volumes:
            - ./postgres-data:/var/lib/postgresql/data
        ports:
            - 5432:5432
```

`env`ファイル

```bash
MB_DB_FILE=/metabase-data/metabase.db
MB_DB_TYPE=postgres
MB_DB_DBNAME=metabase
MB_DB_PORT=5432
MB_DB_USER=postgres
MB_DB_PASS=postgres_user_pass
MB_DB_HOST=postgres-mb

POSTGRES_DB=metabase
POSTGRES_PASSWORD=postgres_user_pass
```

実行

```bash
doker-compose up -d
```

## 03. Getting Started を写経する

[https://www.metabase.com/docs/latest/getting-started.html](https://www.metabase.com/docs/latest/getting-started.html)

### ホームページ

表示されているもの

-   データに基づく自動探査(X-Ray)を試す
-   分析
-   Metabase に接続しているデータベースのリスト

### データを見る

Metabase に添付されている Sample Dataset を使う\
右上の「データを見る」をクリックする\
データの中から Sample Dataset をクリックする ＞ Orders をクリックする ＞ テーブルの中身が表示される

#### 最初の Question

「Orders テーブルの中で、Subtotal 列が\$40 を超える注文は、全部で何行あるか？」が知りたい

##### (1) フィルター

「フィルター」ボタンをクリックして、フィルターのサイドバーを出し、フィルター項目として Subtotal を選択する\
「同等」と表示されているドロップダウンリストを「より大きい」に変更し、テキストボックスに 40 とタイプする\
「フィルターを追加する」ボタンをクリックする

##### (2) 要約

次に、「要約」ボタンをクリックし、サイドバーを出す。デフォルトで「行のカウント」が選択されている\
今回は行数を数えたいので、このまま「完了」ボタンをクリックする\
16,309 行あることがわかった

#### Question を微調整する

続けて、「どの月が一番注文数が多いのか」を知りたい\
再び、「要約」ボタンをクリックし、サイドバーを出す。集約キーに利用できるカラムの一覧がある\
Created At を選択する。即座にグラフが表示される\
中央下にあるトグルをクリックすると、表が登場する。もう一度トグルをクリックすると、グラフに戻る

#### ビジュアルを変更する

左下にある「ビジュアルゼーション」をクリックする。「範囲」をクリックする

### Answer を共有する

最初のステップは、自分の Question を保存することから

#### Question を保存する

右上にある「保存」ボタンをクリックする\
Metabase は、ある程度意味のある名前を先に入力しているが、自分で変更してかまわない\
自分の Question を保存したいフォルダをピックして、保存ボタンをクリックする\
Question を保存すると、Metabase が、「新規もしくは既存のダッシュボードに追加したいか？」を聞いてくる\
新規ダッシュボードを作成してみる。ダッシュボードの名前と説明を入力して作成ボタンをクリックする

### ダッシュボードを作成する

作成ボタンをクリックした後、ダッシュボードの上に、小さなカードになったグラフが見えるはず\
このグラフは、リサイズや移動が自由に行える。右上にある「保存」をクリックする

#### Answer を直に共有する

ダッシュボードの URL をコピーして、メールやチャットに張り付ければ、他の人と共有できる\
共有される人は、Metabase のアカウントを持っていない場合は、作成することになる

## 04. User Guide を勉強する

[https://www.metabase.com/docs/latest/users-guide/start.html](https://www.metabase.com/docs/latest/users-guide/start.html)

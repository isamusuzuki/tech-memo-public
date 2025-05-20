# pgvector

作成日 2025/05/20

## 1. pgvectorとは？

参照サイト => [象使いのための pgvector 入門 (1)](https://qiita.com/hmatsu47/items/b393cecef8ed9df57c35)

> PostgreSQLに、以下を追加するものです。
>
>- ベクトル（vector）データ型
>- ベクトル関数（距離計算など）・オペレータ
>- 近似最近傍探索用のインデックス
>
> DBのテーブルにベクトルデータを保存し、主に最近傍探索をするために使います。
>
>最近よく使われる例として、
>
>- 文章をベクトル化してDBのテーブルに保存する
>- 入力文に近い意味・文脈の文章をテーブルからベクトルを使って検索する
>- テーブルの検索結果を「ヒント」「文脈」のような形で入力文に付け加え、ChatGPTなどのLLM（大規模言語モデル）にプロンプトとして送信する
>
> いわゆるRAG（Retrieval-Augmented Generation）があります。
>
> 文章をベクトル化してベクトル同士を比較する場合は、 似た意味の（文脈が近い）文章が「距離が近い文章」 となり、キーワードの一致がなくとも類似度の高い文書が抽出可能です。

## 2. 公式サイトを探す

ソースコード => [pgvector/pgvector: Open-source vector similarity search for Postgres](https://github.com/pgvector/pgvector)

コンテナ => [pgvector/pgvector - Docker Image | Docker Hub](https://hub.docker.com/r/pgvector/pgvector)

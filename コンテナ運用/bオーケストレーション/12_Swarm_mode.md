# Swarm mode

作成日 2025/01/27

## Swarm mode とは

参照サイト => [Docker Swarm とは？開発終了？特徴から導入手順まで解説](https://and-engineer.com/articles/ZjBUSxAAAHcSq5zj)

> 当初の Docker Swarm は開発を終了し、Docker Engine に統合された Swarm モードがクラスタ管理を行います。
> Swarm モードは、Docker Engine 1.12 以降で組み込まれており、Docker Swarm CLI（コマンドラインインターフェース）で、
> Swarm コンテナを実行したり、ディスカバリトークンを作成したり、クラスタ内のノードを一覧表示したりします。
> Swarm のノードの一覧表示や更新、Swarm からノードの削除など、クラスターマネジメントを行います。

## dockerdocs 日本語版を読む

### [Swarm モード概要](https://matsuand.github.io/docs.docker.jp.onthefly/engine/swarm/)

> 最新版の Docker には Swarm モード が含まれています。
> これは Swarm と呼ばれる Docker Engine のクラスターをネイティブに管理するものです。
> Docker CLI を使って、Swarm の生成、アプリケーションサービスの Swarm へのデプロイ、Swarm の制御管理を行います。

特徴的な機能

- Docker Engine に統合されたクラスター管理
- 分散型設計
- 宣言型サービスモデル
- スケーリング
- 定義状態への調整
- マルチホストネットワーク
- サービス検出
- 負荷分散
- デフォルトで安全
- ローリングアップデート

### [Swarm モードの重要な考え方](https://matsuand.github.io/docs.docker.jp.onthefly/engine/swarm/key-concepts/)

> Swarm とは、複数の Docker ホストが Swarm モード で稼動し、
> マネージャー（参加者を管理し代表となるノード）またはワーカー（Swarm サービス を起動するノード）として振る舞います。
> Docker ホストは、マネージャー、ワーカーのいずれになるか、あるいは両方の役割を持つことができます。

- ノード ... Swarm に参加している Docker Engine のインスタンスの 1 つ
- サービス ... ノード上で実行されるタスク定義のこと
- タスク ... Docker コンテナとその内部で実行されるコマンド。割り当てられたタスクはノードを移動できない

### [Swarm モードをはじめよう](https://matsuand.github.io/docs.docker.jp.onthefly/engine/swarm/swarm-tutorial/)

# Kubernetes とは

作成日 2019/11/17

## 01. k8s 概要

公式トップ => [https://kubernetes.io/](https://kubernetes.io/)

最新バージョン => v1.16

k8s はコンテナ群を複数ノードからなる大規模な環境で管理するのに有用なコンテナオーケストレーションツール

- マスタ ... クラスタ全体の管理を担うコンポーネントを動作させるマシン
- ノード ... コンテナ群を実行するマシン
- クラスタ ... ノードの集合、k8s が管理するマシンの集合
- Pod ... コンテナを管理する単位

1 つのコンテナには 1 つのプロセスのみを含めるのがベストプラクティス

1 つの Pod に含まれるコンテナ群は同一のノード上にデプロイされ、ネットワークスタックやストレージ割当などを共有する

仮想的なネットワークインターフェースは、Pod ごとに割り当てられる。k8s は Pod 起動の際に、クラスタ内で有効な IP アドレスを Pod 単位で払い出す

Pod 内の各コンテナは、仮想的なネットワークインターフェースを共有し、localhost で通信できる

Pod をグループ分けするのが、ラベルやアノテーション

### 「数時間で完全理解！わりとゴツい Kubernetes ハンズオン！！」を読む

[https://qiita.com/Kta-M/items/ce475c0063d3d3f36d5d](https://qiita.com/Kta-M/items/ce475c0063d3d3f36d5d)

## 02. k8s 環境構築

[Kubernetes でクラスタ環境構築手順\(1\) \- master の作成 \- Qiita](https://qiita.com/Esfahan/items/db7a79816731e6aa5cf5)

```bash
sudo yum -y install etcd kubernetes flannel
```

- etcd ... 設定管理、クラスタを構築する
- flannel ... 仮想ネットワーク管理

### etcd とは

[etcd 概要 \- Qiita](https://qiita.com/pocket8137/items/ef44ca68ffc0f4e70995)

etcd => etc distributed => `/etc`に置かれたファイルを分散する

CoreOS の中で提供されている機能のひとつ。CoreOS は Red Hat に買収された

### flannel とは

[coreos/flannel: flannel is a network fabric for containers, designed for Kubernetes](https://github.com/coreos/flannel)

## 03. k8s クラスタを構築するツール

- Tectonic ... CoreOS が提供している k8s ディストリビューションで、10 ノードまでのクラスタは無料
- kubeadm ... k8s が提供している環境構築ツール

[2019 年版・Kubernetes クラスタ構築入門 \| さくらのナレッジ](https://knowledge.sakura.ad.jp/20955/)

[ラズパイで Kubernetes クラスタを構築する](https://qiita.com/sotoiwa/items/e350579d4c81c4a65260)

[【2019 年版】Kubernetes 1\.13 の簡単インストール手順（その１）](https://k8sinfo.com/2019/02/13/create-k8s-environment-manual-1/)

### kubeadm とは

k8s が提供している環境構築ツール

公式トップ => [Installing kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

### Kubespray とは

Ansible を用いてクラスタを一式構築するツール

リポジトリ => [kubernetes\-sigs/kubespray: Deploy a Production Ready Kubernetes Cluster](https://github.com/kubernetes-sigs/kubespray)

[Kubernetes 構築は kubespray がおすすめ！](https://qiita.com/ozota/items/57b0da1cb81d7e6a0762)

公式サイトの Kubespray 説明 => [Installing Kubernetes with Kubespray](https://kubernetes.io/docs/setup/production-environment/tools/kubespray/)

## 04. k8s を前提とした CI/CD パイプライン

[Kubernetes を前提とした CI/CD パイプラインの具体例と、本番運用に必要なもの \(1/2\)](https://www.atmarkit.co.jp/ait/articles/1909/04/news005.html)

> Kubernetes にアプリケーションをデプロイするには、最低でも下記の手順を踏む必要があります。
>
> 1. Docker イメージのビルド
> 2. 「Deployment」「ConfigMap」などの manifest の組み立て（コンテナの image tag や、「dev」「stg」「prod」など各環境に向けた変更）
> 3. 各環境に合わせた config のデプロイ
>
> これらを毎回手動で行うのは、現実的ではありません。そこで、これらの作業を自動化するために、CD 環境が必要になります。
>
> 今回のプロジェクトでは CI/CD ツールとして「Concourse CI」を選択しました。
>
> Concourse CI は Pivotal 社によるオープンソースソフトウェア（OSS）の CI/CD ツールです。yaml を使ってパイプラインを記述し、コンテナベースで各タスクが実行されます。

### Concourse CI とは

[Concourse CI](https://concourse-ci.org/)

> Built on the simple mechanics of resources, tasks, and jobs, Concourse presents a general approach to automation that makes it great for CI/CD.

## 05. Kubernetes Operator とは

### Mattermostのブログで紹介されたこと

[Introducing Mattermost ChatOps: Open source, real\-time DevOps \- Mattermost \- Open Source, On\-Prem or Private Cloud Messaging](https://mattermost.com/blog/introducing-mattermost-chatops/)

>はじめてのアプリケーション向けKubernetes Operatorとして、私たちのオペレータは、シンプルなコマンドでもMattermostのインストールと管理を実現します。Kubernetesの知識はほとんど必要なく、ITにかかる人月を大幅に減らします。ミッションクリティカルなシステムに接続した、セキュリティのきついプライベートネットワークに、Mattermostをスムーズにデプロイできます。
>
>オペレータは、Mattermostを運用にするにあたって、現在、以下の項目をサポートします
>- 高可用性
>- ローリングアップデート
>- 青・緑アップグレード
>- データベース・バックアップ＆移行
>- ユーザー数に応じたスケーリング
>
> operatorhub.ioから、「Mattermostオペレータ」をインストール可能です

[OperatorHub\.io \| The registry for Kubernetes Operators](https://operatorhub.io/operator/mattermost-operator)

[mattermost\-operator/README\.md at master · mattermost/mattermost\-operator](https://github.com/mattermost/mattermost-operator/blob/master/README.md)

### Kubernetes Operatorの解説記事を読む

[あまり良く知られていない Kubernetes Operator とは？ \- Qiita](https://qiita.com/MahoTakara/items/af4ad8ab69c24102bd72)

>オペレーター(Operator)は、アプリケーションなどのサービスを管理している\
>人間のオペレーターの代役を務めることを目的とする。
>
>オペレーターは、カスタム・リソースとカスタム・リソース・コントローラーとして機能する\
>K8s apiserverのクライアントである。

### OperatorHub.ioとは

> 2019年2月末、Red Hatは、Amazon Web Services、Google Cloud、Microsoftと共同でOperatorHub.ioを開始した。OperatorHub.ioは、 Kubernetes Operatorのパブリックレジストリである。 開発者およびK8s管理者にとって、高い品質のオペレーターを見つけることは難しい状況が続いているため、解決の支援となることが期待される。2019年8月現在の総登録数は、66件であった。

### kindとは

kind is a tool for running local Kubernetes clusters using Docker container “nodes”.

公式トップ => [https://kind.sigs.k8s.io/](https://kind.sigs.k8s.io/)

### Mattermost OperatorのREADMEで紹介されていたこと

[mattermost\-operator/README\.md at master · mattermost/mattermost\-operator](https://github.com/mattermost/mattermost-operator/blob/master/README.md)

>To test the operator locally. We recommend Kind, however, you can use Minikube or Minishift as well.

### Kubernetes Operatorの開発環境を整える

まずは、kindとOperator SDKをインストールした環境を用意して、Mattermost Operatorのコードをローカルで動かしてみるのがいいかもしれない

参考記事 => [Go言語で Kubernetes Operator を書いてみた \- Qiita](https://qiita.com/MahoTakara/items/9ce62e8f2226f7b677eb)

- Operator SDKのインストール
- Go言語の開発環境
- Operator-SDKの導入
- オペレータのscaffold作成





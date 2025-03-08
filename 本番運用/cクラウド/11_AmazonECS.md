# Amazon ECS

作成日 2025/01/27、更新日 2025/02/14

フルマネージドコンテナオーケストレーションサービス

公式サイト => [Amazon Elastic Container Service](https://aws.amazon.com/jp/ecs/)

総合サイト => [AWS でのコンテナ](https://aws.amazon.com/jp/containers/)

Getting Started => [Amazon ECS の開始方法](https://aws.amazon.com/jp/ecs/getting-started/)

> Amazon ECS と AWS Fargate を使用すると、ユーザーはミドルウェア、Amazon EC2 インスタンス、またはホスト OS を管理する必要がなくなります。

## コンテナイメージを登録するには

Amazon ECR プライベートリポジトリを使う

ユーザーガイド => [Amazon Elastic Container Registry とは](https://docs.aws.amazon.com/ja_jp/AmazonECR/latest/userguide/what-is-ecr.html)

1. [イメージを保存するための Amazon ECR プライベートリポジトリの作成](https://docs.aws.amazon.com/ja_jp/AmazonECR/latest/userguide/repository-create.html)
1. [IAM Amazon ECR プライベートリポジトリにイメージをプッシュするための アクセス許可](https://docs.aws.amazon.com/ja_jp/AmazonECR/latest/userguide/image-push-iam.html)
1. [Amazon ECR プライベートリポジトリへの Docker イメージのプッシュ](https://docs.aws.amazon.com/ja_jp/AmazonECR/latest/userguide/docker-push-ecr-image.html)
1. [Amazon ECS での Amazon ECR イメージの使用](https://docs.aws.amazon.com/ja_jp/AmazonECR/latest/userguide/ECR_on_ECS.html)

## [Amazon ECS Anywhere](https://aws.amazon.com/jp/ecs/anywhere/)

- Create activation key
- Install SSM and ECS agent
- Define your application
- Deploy and manage container applications

[Amazon ECS Anywhere の料金](https://aws.amazon.com/jp/ecs/anywhere/pricing/)

> マネージド ECS Anywhere オンプレミスインスタンスごとに、1 時間あたり 0.01025 USD をお支払いいただきます。

24 hours x 30 days = 720 hours x 0.01025 USD = 7.38 USD x 150 YEN = 1,107 YEN

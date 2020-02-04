---
tags: devtools
---

# CI/CDツールの紹介

作成日 2019/04/16

## 01. Jenkins

Javaで書かれたCode Integrationツール。ソフトウェアのビルド、検証、サーバーへのインストール等の一連作業を自動化できる

公式トップ => [https://jenkins.io/](https://jenkins.io/)

### Jenkins X

Kubernetesに特化したCI/CDツール

[「Jenkins X」発表。Git/Docker/Kubernetesに特化したことでCI/CD環境の構築運用を自動化](http://www.publickey1.jp/blog/18/jenkins_xgitdockerkubernetescicd.html)

[Introducing Jenkins X: a CI/CD solution for modern cloud applications on Kubernetes](https://jenkins.io/blog/2018/03/19/introducing-jenkins-x/)

## 02. CircleCI

公式トップ => [https://circleci.com/](https://circleci.com/)

### CircleCIの紹介記事

[CircleCIを使って「完全サーバーレス運用」を実現した話](https://tech.bizreach.co.jp/posts/254/circleci/)]

## 03. DeployBot

複数のリポジトリに対応したクラウドのCDツール

公式トップ => [https://deploybot.com/](https://deploybot.com/)

> Instantly build and ship code anywhere in one consistent process for your entire team.

### DeployBotの料金プラン

- Basic Plan $15/month ... 300 servers 10 repositories
- Plus Plan $25/month ... 600 servers 20 repositories
- Premium Plan $50/month ... 1,000 servers 50 repositories

### DeployBotの対応リポジトリ

- GitHub, Bitbucket, GitLab ... Any git repository
- Manual or automatic deployments
- Tools for multiple environments

### DeployBotの対応機能＆サービス

- Fetch and deploy dependencies ... npm, node.js, composer
- Comile code ... Java, Scala, Golang
- Build & compress CSS, JS, and images ... Gulp, Grunt, Sass, CoffeeScript
- Deploying Code Changes ... FTP/SFTP, AWS, DigitalOcean, heroku, shopify
- Get notified ... SMS, Slack, Campfire
- Monitor Deployments ... New Relic, bugsnag

## 04. Bitbucket Pipelines

- Bitbucketに統合されている
- ビルドとテストを自動化
- デプロイメントの確実性を保証

### BitBucket Pipelinesのドキュメント

[Build, test, and deploy with Pipelines \- Atlassian Documentation](https://confluence.atlassian.com/bitbucket/build-test-and-deploy-with-pipelines-792496469.html)

[PHP with Bitbucket Pipelines \- Atlassian Documentation](https://confluence.atlassian.com/bitbucket/php-with-bitbucket-pipelines-873907835.html)

[Use SSH keys in Bitbucket Pipelines \- Atlassian Documentation](https://confluence.atlassian.com/bitbucket/use-ssh-keys-in-bitbucket-pipelines-847452940.html)

[Deploy to Amazon AWS \- Atlassian Documentation](https://confluence.atlassian.com/bitbucket/deploy-to-amazon-aws-875304040.html)

### BitBucket Pipelinesの参考記事

[BitbucketのPipelinesを試してみる \- Qiita](https://qiita.com/yshishido/items/b3a57ffeae708841980d)




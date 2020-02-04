---
tags: mattermost
---

# Mattermost管理者ガイド

作成日 2019/06/03

[https://docs.mattermost.com/guides/administrator.html](https://docs.mattermost.com/guides/administrator.html)

## 01. Getting Started

Implementation Plan

- 3 つのエディションがある。Team Edition/Enterprise Edition(E10)/Enterprise Edition(E20)
- Mattermost Server は、シングルバイナリー
- Proxy サーバー (NGINX 他) の使用は推奨。メリット: SSL 終端、HTTPS リダイレクション、ポートマッピング、ログ生成

Implementation Schedule

1. Create System Architecture Document
1. Gather Required Software and Document
1. Prepare Deployment Environments
1. Install Software
1. Test Deployment
1. Bulk Load Data
1. Implement Backup
1. Implement Monitoring
1. Train Administrators
1. Onboard Users

## 02. Installing Mattermost

Software Requirements > Server Software

- OS は、実質 Ubuntu と CentOS からの二択になる
- DB は、MySQL と PostgreSQL からの二択になる
- 日本語検索の導入には、`ngram Full-Text parser` を設定するので、MySQL 5.7.6 以上が必要
- 2 文字の検索には、追加設定が必要（デフォルトでは 2 文字はできないということか？）
- MySQL では、ハッシュタグや最近のメンションの検索結果に、ドット入りのユーザー名が含まれない（ユーザー名にドットを入れるなということか？）
- MySQL 8 を使うときは、認証プラグインが変更になっているので、設定の追加が必要

Hardware Requirements for Team Deployments

- 1 から 1,000 ユーザーまで ... 1 vCPU/core, 2GB RAM
- 1,000 から 2,000 ユーザーまで ... 2 vCPUs/cores, 4GB RAM

Alternate Storage Calculations

- まず最初に、OS と DB に、600~800MB を見込む
- 後述する `Estimated storage per user per month` を 12 倍して 1 年分にする
- さらに、年間の平均ユーザー数をかける
- 安全要素として、1~2 倍する

Estimated storage per user per month

- 使用量が低いユーザー　... 1-5MB/user/month
- 使用量が中程度のユーザー ... 5-25MB/user/month
- 使用量が高いユーザー ... 25-100MB/user/month

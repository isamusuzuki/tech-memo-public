# Grafana Fundamentals

作成日 2025/12/25

[Grafana fundamentals | Grafana Labs](https://grafana.com/tutorials/grafana-fundamentals/)

## サンプルアプリケーションをセットアップする

```bash
# リポジトリからクローンする
git clone https://github.com/grafana/tutorial-environment.git

cd tutorial-environment

# Dockerが起動していることを確認する
docker ps

# サンプルアプリを起動する
docker-compose up -d

# 全てのサービスが起動中であることを確認する
docker-compose ps
```

## Grafana News

ブラウザで`http://localhost:8081`を開く

Grafana Newsという画面では、リンクを登録し、投票できるようになっている

タイトルにExampleと入力し、URLに`https://example.com`と入力し、Submitボタンをクリックする

投票するには、三角形アイコンをクリックする

## Grafana管理画面

``<http://localhost:3000`>

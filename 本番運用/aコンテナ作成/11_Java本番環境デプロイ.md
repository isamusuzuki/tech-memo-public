# [Java 本番環境へのデプロイ手順書](https://zenn.dev/teru_whisky/books/4f6edb84657449)

作成日 2025/01/21

## 1. はじめに

- JAR ファイル ... シンプル軽量、コマンドラインで実行可能
- WAR ファイル ... Web アプリケーション向け、サーバーにデプロイ

## 2. JAR ファイルを使用したデプロイ

サーバーで JAR ファイルを実行するには、以下のコマンドを使います。

```bash
java -jar app.jar
```

アプリケーションが正しく動作しているか確認するために、実行中に出力されるログを確認します。

```bash
java -jar app.jar > logs/app.log 2>&1 &
```

## 3. WARファイルを使用したデプロイ

略

## 4. クラスファイルをそのまま使用したデプロイ

略

## 5. コンテナ化されたデプロイ

Dockerfile の作成

```dockerfile
# 基盤となるイメージを指定
FROM openjdk:11-jre-slim

# 作業ディレクトリの設定
WORKDIR /app

# JARファイルをコンテナにコピー
COPY target/myapp.jar myapp.jar

# アプリケーションの実行コマンド
CMD ["java", "-jar", "myapp.jar"]
```

Docker イメージのビルド

```bash
docker build -t myapp .
```

Docker コンテナの起動

```bash
docker run -d -p 8080:8080 myapp
```

## 6. AWSを使用したクラウドベースのデプロイ

AWS Elastic Beanstalkを使用したデプロイ

```bash
eb init -p java my-app # プロジェクトの初期化
eb create my-env       # 環境の作成
eb deploy              # アプリケーションのデプロイ
```

## 7. まとめと補則

略

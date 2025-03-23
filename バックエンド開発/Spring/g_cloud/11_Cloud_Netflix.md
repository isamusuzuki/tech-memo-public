# Spring Cloud Netflix

作成日 2025/02/13

## 参照サイト

- [Netflix Eureka サービスディスカバリ](https://spring.pleiades.io/guides/gs/service-registration-and-discovery)

まず、Eureka サーバーが必要です。Spring Cloud の @EnableEurekaServer を使用して、他のアプリケーションが通信できるレジストリを立ち上げることができます。これは、サービスレジストリを有効にするために 1 つのアノテーション (@EnableEurekaServer) が追加された通常の Spring Boot アプリケーションです。

## プロジェクトページ（英語）を読む

[Spring Cloud Netflix](https://spring.io/projects/spring-cloud-netflix)

Eureka クライアント

- `spring-cloud-starter-netflix-eureka-client` in dependencies
- `http://localhost:8761`の Eureka サーバーとコンタクトしようとする

Eureka サーバー

- `spring-cloud-starter-netflix-eureka-server` in dependencies
- `@EnableEurekaServer` to Application class

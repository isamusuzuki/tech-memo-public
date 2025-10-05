# Validation

作成日 2025/02/13

公式サイト => [Jakarta Bean Validation](https://beanvalidation.org/)

開発ガイド（古い） => [第 13 章 Jakarta Bean Validation](https://docs.redhat.com/ja/documentation/red_hat_jboss_enterprise_application_platform/7.4-beta/html/development_guide/jakarta_bean_validation)

> Jakarta Bean Validation は、Java オブジェクトのデータを検証するモデルです。このモデルでは、組み込みのカスタムアノテーション制約を使い、アプリケーションデータの整合性を保ちます。また、メソッドおよびコンストラクターバリデーションも提供し、パラメーターおよび戻り値の制約を確保します。

## apidocs を見る

[Package jakarta.validation.constraints](https://jakarta.ee/specifications/bean-validation/3.0/apidocs/)

- @Digits(fraction=, integer=)
- @Email
- @Future
- @NotNull
- @Past
- @Size(min=, max=)

[Annotation Type Valid](https://jakarta.ee/specifications/bean-validation/3.0/apidocs/)

- @Valid ... 紐付けされたオブジェクトに再帰的にバリデーションを実行する

## セットアップ

[Spring Initializr](https://start.spring.io/)の Dependencies にあった

> Validation [I/O]
> Bean Validation with Hibernate validator.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

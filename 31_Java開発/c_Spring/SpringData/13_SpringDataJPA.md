# Spring Data JPA

作成日 2025/02/12

参照書籍 => P.506 "10.4 Spring Data JPA のアーキテクチャ" from 「Spring 徹底入門 第 2 版」

## 内部処理の流れ

- Spring Data JPA は、JPA の EntityManager の API をラップして、利用者に対して隠ぺいする
- 利用者は、Spring Data JPA が提供する API を呼び出す
- EntityManager の API 呼び出しを行っているコード => SimpleJpaRepository クラス
- 利用者は、Entity 専用の Repository インターフェイスを作成する
- Repository インターフェイスには、Spring Data JPA が提供する JpaRepository インターフェイスを継承させる
- JpaRepository インターフェイスは、SimpleJpaRepository クラスが提供する API をインターフェイス化したもの
- アプリケーションが Entity に対してデータアクセスする場合は、対応する Repository インターフェイスを介す
- しかし、このままでは、利用者が作成した Repository と SimpleJpaRepository クラスの間に直接的な結び付きがない
- Spring Data JPA は、DI コンテナ初期化時に、Repository に対して SimpleJpaRepository へ処理を移譲するような Proxy クラスを生成し、\
  そのインスタンスを DI コンテナに Bean として登録する
- アプリケーションには、Proxy クラスが挿入され、そのメソッドを実行することで、SimpleJpaRepository の呼び出しが実現される

Proxy パターンによるデザインは、スーパークラスを 1 つしか持てないという Java の継承の制約を回避している

## JpaRepository が提供する API

- count()
- findById(id)
- findAll()
- findAll(sort)
- findAllById(ids)
- findAll(pageable)
- save(entity)
- saveAll(entities)
- flush()
- saveAndFlush(entity)
- delete(entity)
- deleteAll(entities)
- deleteById(id)
- deleteAll()
- deleteInBatch(entities)
- deleteAllInBatch()

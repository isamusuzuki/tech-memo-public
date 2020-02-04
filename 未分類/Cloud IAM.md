---
tags: gcp
---

# Cloud IAM

作成日 2019/12/17

## 01. Cloud IAMとは

IAM = Identity and Access Management

ドキュメント => [Cloud Identity and Access Management のドキュメント](https://cloud.google.com/iam/docs/?hl=ja)

### Overviewを読む

[Overview  \|  Cloud IAM Documentation](https://cloud.google.com/iam/docs/overview)

英語と日本語を行き来しながら読む

### Members

- Google account ... 開発者、管理者、GCP を操作する人間
- Service account ... 個々のエンドユーザーというよりもアプリケーションに属する
- Google group ... Google account と Service account の集合
- G Suite domain ... 組織アカウントである Google account の仮想的なグループ
- Cloud Identity domain ... G Suite アプリケーションにアクセスできない、仮想グループ
- allAuthenticatedUsers ... 認証された Google account と Service account の全て
- allUsers ... 認証されたものとされていないものを含むすべて

### Resource

- projects
- Compute Engine instances
- Cloud Storage buckets

### Permissions（権限）

`<service>.<resource>.<verb>` => pubsub.subscriptions.consume

### Roles（役割）

- Primitive roles ... 基本の役割（オーナー、編集者、閲覧者）
- Predfined roles ... 事前定義済みの役割（Pub/Subパブリッシャーなど）
- Custom roles ... カスタムの役割

### IAM Policy

```json=
{
  "bindings": [
    {
      "role": "roles/storage.objectAdmin",
      "members": [
        "user:alice@example.com",
        "serviceAccount:my-other-app@appspot.gserviceaccount.com",
        "group:admins@example.com",
        "domain:google.com"
      ]
    },
    {
      "role": "roles/storage.objectViewer",
      "members": ["user:bob@example.com"]
    }
  ]
}
```

## 02. 役割(Role)について

[役割について  \|  Cloud IAM のドキュメント](https://cloud.google.com/iam/docs/understanding-roles?hl=ja#primitive_roles)

各プロジェクトには、「オーナー(roles/owner)」「編集者(roles/editor)」「閲覧者(roles/viewer)」という「基本の役割」が存在する

新しくCloud Functionsをデプロイすると、"project-name@appspot.gserviceaccount.com"というサービスアカウントが付与される。これは、App Engine default service accountと説明されているが、Cloud Functionsのデフォルトでもある

このサービスアカウントには「編集者」が与えられているので、そのプロジェクト内では、ほぼ何でもできる




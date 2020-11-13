# Cloud IAM を学ぶ

作成日 2020/02/13

## 01. IAM とは

IAM = Identity and Access Management

ドキュメント => [Cloud Identity and Access Management のドキュメント](https://cloud.google.com/iam/docs/?hl=ja)

### Overview を読む

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
- Predfined roles ... 事前定義済みの役割（Pub/Sub パブリッシャーなど）
- Custom roles ... カスタムの役割

### IAM Policy

```json
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

### 基本の役割

各プロジェクトには、「オーナー(roles/owner)」、「編集者(roles/editor)」、「閲覧者(roles/viewer)」という「基本の役割」が存在する

| name         | role                                                     |
| ------------ | -------------------------------------------------------- |
| roles/viewer | 状態に影響しない読み取り専用アクションに必要な権限       |
| roles/editor | 上記＋状態を変更するアクションに必要な権限               |
| roles/owner  | 上記＋リソースの権限と役割を管理する、課金情報を設定する |

新しく Cloud Functions をデプロイすると、\
`project-name@appspot.gserviceaccount.com` というサービスアカウントが付与される

これは、App Engine default service account と説明されているが、\
Cloud Functions のデフォルトでもある

このサービスアカウントには「編集者」が与えられているので、\
そのプロジェクト内では、ほぼ何でもできる

## 03. gcloud コマンドを使って管理する

公式ドキュメント => [https://cloud.google.com/iam/?hl=ja](https://cloud.google.com/iam/?hl=ja)

### gcloud コマンドで、現在の許可を確認する

```bash
gcloud projects get-iam-policy project-name

# 読み切れないので、いったんファイルに保存する
gcloud projects get-iam-policy project-name > iam.yaml
```

#### 自分のアカウントが持っている role を洗い出す

```yaml
- members:
    - user:taro@example.com
  role: roles/cloudscheduler.admin
- members:
    - user:taro@example.com
  role: roles/editor
- members:
    - user:taro@example.com
  role: roles/iam.securityAdmin
- members:
    - user:taro@example.com
  role: roles/source.admin
```

### gcloud コマンドで、許可を付与する

```bash
gcloud projects add-iam-policy-binding project-name \
  --member user:taro@example.com \
  --role roles/appengine.appAdmin
```

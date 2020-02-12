# gcloud コマンド

作成日 2019/12/06、更新日 2019/12/23

## 01. gcloud コマンドとは

Google Cloud Platform を利用するのに必須のコマンドラインツール\
"Google Cloud SDK"という枠組の中のひとつのツール

公式トップ => [https://cloud.google.com/sdk/?hl=ja](https://cloud.google.com/sdk/?hl=ja)

## 02. gcloud コマンドをインストールする

バージョン 274.0.0 から、gcloud コマンドの Python3.5 系以降での\
動作がGA サポートとなった\
しかし、インストーラーによるインストールがふたたび失敗したので、\
以下に述べる「アーカイブからのインストール」を継続利用する

### 「アーカイブからのインストール」手順

-   [バージョニングされたアーカイブからのインストール](https://cloud.google.com/sdk/docs/downloads-versioned-archives)
-   ページに掲載されているバージョン 245 は古いので、アーカイブページから一番新しいバージョンを探す
-   [Google Cloud Storage のダウンロードアーカイブ](https://console.cloud.google.com/storage/browser/cloud-sdk-release?authuser=0)
-   検索窓に、`google-cloud-sdk-nnn`と入力して、意中のバージョンを探す
-   「windows 64 ビット版」が欲しいので、末尾が`windows-x86_64.zip`になっているものをクリックする
-   リンク URL をクリックして、ダウンロードする
-   ZIP の中の google-cloud-sdk フォルダをホームフォルダに解凍する。前のバージョンがある場合は、あらかじめフォルダごと削除しておく

### ユーザー環境変数を設定する

| 処理       | 変数名          | 変数値                               |
| ---------- | --------------- | ------------------------------------ |
| 新規に作成 | CLOUDSDK_PYTHON | `C:\Python37\python.exe`             |
| 末尾に追加 | Path            | `%USERPROFILE%\google-cloud-sdk\bin` |

### インストールの確認

```bash
gcloud --version
#=> Googgle Cloud SDK 270.0.0
#=> bq 2.0.49
#=> core 2019.11.04
#=> gsutil 4.46

# インストール済みのコンポーネントの確認
gcloud components list

# セットアップ作業（アカウント認証とプロジェクト選択）
gcloud init
```

### gcloud コマンドの更新

`gcloud compnents update`コマンドは、Git Bash では動作しない\
必ず PowerShell で実行すること

## 03. アカウントとプロジェクトを管理する

### アカウント（OAuth2 認証）を管理する

```bash
gcloud auth login
gcloud auth list
gcloud auto revoke
```

### アカウントとプロジェクトの設定を変更する

```bash
gcloud config list
gcloud config set project project1
gcloud config set account account1@example.com
```

### プロジェクトとアカウントをセットにして管理する

```bash
gcloud config configurations create config2
gcloud config set account account2@example.com
gcloud config set project project2

gcloud config configurations list
# NAME     IS_ACTIVE ACCOUNT                       PROJECT
# default  False     account1@example.com          project1
# config2  True      account2@example.com          project2

gcloud config configurations activate default
#=> Activated [default].

gcloud config configurations delete config2
#=> Deleted [config2].
```

## 04. アカウント権限を管理する

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

#### role について勉強する

[役割について](https://cloud.google.com/iam/docs/understanding-roles?hl=ja)

基本の役割

| name         | role                                                     |
| ------------ | -------------------------------------------------------- |
| roles/viewer | 状態に影響しない読み取り専用アクションに必要な権限       |
| roles/editor | 上記＋状態を変更するアクションに必要な権限               |
| roles/owner  | 上記＋リソースの権限と役割を管理する、課金情報を設定する |

### gcloud コマンドで、許可を付与する

```bash
gcloud projects add-iam-policy-binding project-name \
--member user:taro@example.com \
--role roles/appengine.appAdmin
```

## 05. Cloud Functions を管理する

リファレンス => [https://cloud.google.com/sdk/gcloud/reference/functions/deploy](https://cloud.google.com/sdk/gcloud/reference/functions/deploy)

```bash
# デプロイ済みの関数の一覧を表示する
gcloud functions list

#  関数をデプロイする
gcoud functions deploy NAME\
  --region REGION
  --memory MEMORY
  --timeout TIMEOUT
  --runtime RUNTIME
  TRIGGER
  --env-vars-file FILE_PATH
```

-   NAME ... 関数名。`--entry-point`で別途指定しない限り、同名の関数名が必要
-   REGION ... `asia-northeast1`など。省略したら`us-entral1`
-   MEMORY ... `256MB`, `512MB`, `1024MB`など。省略したら`256MB`
-   TIMEOUT ... 30 秒は`30s`と書く。540 秒以上は設定できない。省略したら 60 秒
-   RUNTIME ... `python37`, `nodejs10`のいずれか
-   TRIGGER ... `--trigger-http`, `--trigger-topic`, `--trigger-bucket`, `--trigger-event --trigger-resource` のいずれか
-   FILE_PATH ... `.env.yaml`ファイルを指定すると、環境変数として取り込まれる

## 06. Cloud Source Repositories を管理する

公式トップ => [https://cloud.google.com/source-repositories/?hl=ja](https://cloud.google.com/source-repositories/?hl=ja)

クイックスタート => [https://cloud.google.com/source-repositories/docs/quickstart?hl=ja](https://cloud.google.com/source-repositories/docs/quickstart?hl=ja)

```bash
# 新しいリポジトリを作成する
gcloud source repos create hello-world --project=project-123456

# リポジトリのクローンを作成する
gcloud source repos clone hello-world --project=project-123456

# 作成したファイルをリモートにプッシュする
git add .
git commit -m 'message'
git push origin master
```

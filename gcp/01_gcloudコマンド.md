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

`gcloud components update`コマンドは、Git Bash では動作しない\
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

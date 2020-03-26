# Cloud Source Repositories を使う

作成日 2020/03/27

## 01. gcloud コマンドで管理する

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

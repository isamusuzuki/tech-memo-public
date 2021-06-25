# よく使う Git コマンド

作成日 2021/06/25

## 01. 新しいブランチを作成して切り替える

```bash
# issue1というブランチを作成する
git branch issue1

# ブランチの一覧を表示する
git branch
#=> *master
#=>  issue1

# ブランチを切り替える
git checkout issue1

git branch
#=>  master
#=> *issue1

# issue1で行った変更をmasterに取り込ませる
git checkout master
git merge issue1

# ブランチを削除する
git branch issue1 --delete
```

## 02. コミットを取り消す

```bash
# 直前のコミットを取り消す（コミットのみ取り消す）
git reset --soft HEAD^

# 直前のコミットを取り消す（まったくなかったことにする）
# 新規ファイルは消えるし、編集内容も失われるので要注意
git reset --hard HEAD^

# リモートにまでプッシュしていた場合は
# 強制的に同調させる
git push -f origin master
```

## 03. 手元にないリモートブランチを取得する

```bash
# リモートからデータをすべて取得する
git fetch origin

# originのdevelopブランチを追跡するローカルのdevelopブランチを作成する
git branch develop origin/develop

# ブランチを切り替える
git checkout develop
```

## 04. ローカルをリモートに強制一致させる

```bash
# リモートからデータをすべて取得する
git fetch origin

# ローカル内で状態を動かす
git reset --hard origin/master
```

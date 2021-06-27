# git branch コマンド

作成日 2021/06/27

- `git branch`  ... ブランチの一覧を表示する
- `git branch <name>`  ... 新しいブランチを作成する
- `git branch <name> --delete`  ... ブランチを削除する
- `git branch <name> <remote/name>` ... remoteの指定ブランチを追跡するブランチを作成する

## 新しいブランチを作成して切り替える

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

## 手元にないリモートブランチを取得する

```bash
# リモートからデータをすべて取得する
git fetch origin

# originのdevelopブランチを追跡するローカルのdevelopブランチを作成する
git branch develop origin/develop

# ブランチを切り替える
git checkout develop
```

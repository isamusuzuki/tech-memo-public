# git branch コマンド

作成日 2021/06/27

- `git branch` ... ローカルにあるブランチを表示する
- `git branch -r(--remote)` ... リモートにあるブランチを表示する
- `git branch -a(--all)` ... ローカル・リモートすべてのブランチを表示する
- `git branch {branch}` ... 新しいブランチを作成する
- `git branch {branch} -d(--delete)` ... ブランチを削除する
- `git branch {branch} {remote/branch}` ... remote の指定ブランチを追跡するブランチを作成する

## 新しいブランチを作成して切り替える

```bash
# ブランチの一覧を表示する
git branch
#=> *main 

# issue1というブランチを作成する
git branch issue1

# ブランチの一覧を表示する
git branch
#=> *main
#=>  issue1

# ブランチを切り替える
git checkout issue1

git branch
#=>  main
#=> *issue1
```

## あるブランチで行った変更履歴を別のブランチに取り込ませる

```bash
# issue1で行った変更をmainに取り込ませる
git checkout main
git merge issue1

# ブランチを削除する
git branch issue1 -d
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

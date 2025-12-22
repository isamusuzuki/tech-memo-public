# git fetch コマンド

作成日 2021/06/30

- `git fetch {remote}` ... 指定したリモートリポジトリの情報を取得する
- `git fetch {remote} -p(--prune)` ... リモートで削除されたブランチはローカルでも削除する

## 手元にないリモートブランチを取得する

```bash
# リモートからデータをすべて取得する
git fetch origin

# originのdevelopブランチを追跡するローカルのdevelopブランチを作成する
git branch develop origin/develop

# ブランチを切り替える
git checkout develop
```

## ローカルをリモートに強制一致させる

```bash
# リモートからデータをすべて取得する
git fetch origin

# ローカル内で状態を動かす
git reset --hard origin/master
```

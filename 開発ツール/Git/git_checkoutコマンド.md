# git checkout コマンド

作成日 2021/07/20

- `git checkout {branch}` ... ローカルのブランチの中で切り替える

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

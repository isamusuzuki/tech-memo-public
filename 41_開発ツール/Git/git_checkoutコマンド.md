# git checkout コマンド

作成日 2021/07/20、更新日 2025/11/19

- `git checkout {branch}` ... ローカルのブランチの中で切り替える

```bash
# ブランチの一覧を表示する
git branch
#=> *main

# issue1というブランチを作成し、同時に切り替える
# bオプションの後にブランチ名が来る必要あり
git checkout -b issue1

# ブランチの一覧を表示する
git branch
#=> main
#=> *issue1

# ブランチを切り替える
git checkout main

git branch
#=> *main
#=> issue1
```

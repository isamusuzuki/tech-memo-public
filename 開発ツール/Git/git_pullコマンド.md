# git pull コマンド

作成日 2021/07/18、更新日 2025/04/01

- `git pull {remote} {branch}` ... リモートブランチの変更内容を取り込む

```bash
# origin/main ブランチの変更履歴を取り込む
git pull origin main
```

## よくやる失敗

ローカルにないリモートブランチを取り込もうと思ってプルしてはいけない

現在のローカルブランチにリモートブランチのデータが積み重なってしまう

```bash
# やってはいけない
# git pulll origin other-dev

# ただしいコマンド
git branch other-dev origin/other-dev
git checkout other-dev
```

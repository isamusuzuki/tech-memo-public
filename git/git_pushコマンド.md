# git push コマンド

作成日 2021/07/18

- `git push {remote} {branch}` ... ローカルブランチの変更内容を指定したリモートブランチに書き込む

```bash
git push origin main

# 初回連携用
# 指定したリモートブランチを上流ブランチとして設定する
git push origin main -u

# 指定したブランチをリモートから削除する
git push origin 2107_work -d
```

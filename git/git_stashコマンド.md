# git stash コマンド

作成日 2023/01/31

`git stash`  ... コミットしていない変更を退避させる

## 変更を退避させたり、戻したりする

```bash
# 追跡対象に含まれていない新規作成ファイルも含めて、退避させる
git stash -u (--include-untracked)

# 退避させた直近の作業を戻す
git stash apply

# 退避させた直近の作業を消す
git stash drop

# 退避させた作業の一覧を見る
git stash list

# 退避させた作業を戻す（カッコ内の数字は流動的）
git stash apply stash@{0}

# 退避させた作業を消す（カッコ内の数字は流動的）
git stash drop stash@{0}
```

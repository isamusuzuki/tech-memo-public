# git stash コマンド

作成日 2023/01/31、更新日 2025/01/17

## 変更を退避させる

```bash
# コミットしていない変更を退避させる
git stash
git stash push

# メッセージをつけて退避させる
git stash "message"
git stash push "message"

# 追跡対象に含まれていない新規作成ファイルも含めて、退避させる
git stash -u
git stash push -u
```

## 退避させたものを確認する

```bash
# 退避させた一覧を見る
git stash list

# N番目に退避させたファイルの一覧を表示する
git stash show stash@{N}

# N番目に退避させたファイルの変更差分を表示する
git stash show -p stash@{N}
```

## 退避させたものを元に戻す

```bash
# 直近の退避させたものを戻し、削除する
git stash pop

# 直近の退避させたものを戻し、一覧に残す
git stash apply

# N番目に退避させたものを戻し、削除する
git stash pop stash@{N}

# N番目に退避させたものを戻し、一覧に残す
git stash apply stash@{N}
```

## 退避させたものを削除する

```bash
# 直近の退避させたものを削除する
git stash drop

# N番目に退避させたものを削除する
git stash drop stash@{N}

# 退避させた一覧を全て削除する
git stash clear
```

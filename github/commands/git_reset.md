# git reset コマンド

作成日 2021/06/27

- `git reset <id>`  ... 指定したコミットまで手戻りする
- `git reset <remote/name>` ... リモートのブランチに手戻りする（強制一致させる）

## コミットを取り消す

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

## 04. ローカルをリモートに強制一致させる

```bash
# リモートからデータをすべて取得する
git fetch origin

# ローカル内で状態を動かす
git reset --hard origin/master
```

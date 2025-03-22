# git rebase コマンド

作成日 2022/08/04、更新日 2025/03/09

- `git rebase {branch}` ... 繋ぐ元にするブランチ名を指定して繋ぐ
- `git rebase -i(--interactive)` ... Git の歴史を改変する

## 1. 開発コミットを繋げ直す

```bash
git checkout dev
git rebase main
```

## 2. 複数の commit をまとめる

エディタが 2 回立ち上がる。1 回目で「どのコミットに対して何をしたいのか」を指示する\
2 回目で「新しいコミットメッセージ」を指示する。エディタは PowerShell でも vim であった

コミットのログを確認する（新しい順に表示される）

```bash
git log --oneline -3
# 04183ba (HEAD -> develop) bar
# c55e5e9 foo
# 1e57b68 hoge
```

作業を始める

```bash
git rebase -i HEAD~2
```

エディタが立ち上がり、古い順に表示される

```bash
# ----- 修正前のエディタ画面 -----
pick c55e5e9 foo
pick 04183ba bar
```

まとめたいコミットの pick を s(squash の略、「押しつぶす」という意味)に変更して\
エディタを保存して閉じる。まとめられるコミットは自分の前のコミットに吸収される\
「上を残して、下を消す」

```bash
# ----- 修正後のエディタ画面 -----
pick c55e5e9 foo
s 04183ba bar
```

2 回目のエディタが立ち上げる

```bash
# ----- 修正前のエディタ画面 -----
# This is a combination of 2 commits.
# This is the 1st commit message:

foo

# This is the commit message #2:

bar
```

残す方のコミットメッセージを変更して、まとめられる方のコミットメッセージを空にする

```bash
# ----- 修正後のエディタ画面 -----
# This is a combination of 2 commits.
# This is the 1st commit message:

foo & bar

# This is the commit message #2:


```

新しいコミットメッセージで新しいコミットが作られた事を確認する

```bash
git log --oneline -2
# 342db33 (HEAD -> develop) foo & bar
# 1e57b68 hoge
```

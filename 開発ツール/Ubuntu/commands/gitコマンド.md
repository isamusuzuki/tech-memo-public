# git コマンド

作成日 2020/03/30、更新日 2020/04/02

## 01. いったん昔の状態に戻る

git リポジトリの中には、昔のファイルがすべて残っている\
一度コミットされたファイルは取り出すことが可能

Github なり、Source Repositories なり、プロジェクトページに行って\
コミットヒストリーの中から、適当なコミット ID をコピペしておく

```bash
# 指定したコミットに状態を戻す
git reset --hard COMMIT_ID

# 元に（先頭に）戻る
git pull origin master
```

## 02. 遡って（さかのぼって）ブランチ分けする

例えば、master ブランチだけで開発していて、ちょっと「進みすぎた」とする\
「進みすぎた」というのは、ある機能を開発していて、何個かコミットしてきたけれども\
リリースは時期尚早で、その前にバグフィックスをしたい思ったときのことである\
こんな時は、遡ってブランチ分けしておくのがいい

例えば、以下のようにコミットを重ねてきたとする

| commit id | commit date      |
| :-------: | ---------------- |
| `88ab868` | 2020/03/27 14:00 |
| `682f226` | 2020/03/27 13:00 |
| `6c4b9b7` | 2020/03/27 11:00 |
| `c862f9d` | 2020/03/23 16:00 |
| `3ed6f84` | 2020/03/23 15:00 |

`c862f9d` は、本番サーバーにデプロイしてあるとする\
その後、3 回コミットしてきたが、開発した新機能はまだデプロイできない\
その前に、軽微なバグフィックスを、デプロイする必要がでてきた

```bash
# 過去の状態に戻る
git reset --hard c862f9d
# => HEAD is now at c862f9d

# 過去の状態で、devブランチを作成する
git branch dev
git checkout dev
# => Switched to branch 'dev'

# devブランチで、チェリーピックを3回行う
git cherry-pick 6c4b9b7
git cherry-pick 682f226
git cherry-pick 88ab868

# サーバーにdevブランチをプッシュする
git push origin dev
# =>  * [new branch]      dev -> dev

# masterブランチに戻る
git checkout master
# => Switched to branch 'master'
# => Your branch is behind 'origin/master' by 3 commits, and can be fast-forwarded.
# =>   (use "git pull" to update your local branch)

# サーバーのmasterブランチを強制的に後戻りさせる
git push -f origin master
# => Total 0 (delta 0), reused 0 (delta 0)
# => To https://source.developers.google.com/p/your-project/r/your-repository
# =>  + 88ab868...c862f9d master -> master (forced update)
```

## サーバーにプッシュまでしたあとに、コミットをやり直す

直近2回のコミットをまとめたいと思ったとき

```bash
# 変更は残したまま２つ前の状態に戻る
git reset --soft HEAD~2

# サーバーに２回分のコミットを忘れさせる
git push -f origin master

# あらためてコミットする
git add .
git commit -m 'comment'

# 改めてサーバーにプッシュする
git push origin master
```

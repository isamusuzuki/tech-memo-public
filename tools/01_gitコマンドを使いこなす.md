# git コマンドを使いこなす

作成日 2020/03/27

## 01. いったん昔の状態に戻る

git リポジトリの中には、昔のファイルがすべて残っている\
一度コミットされたファイルは取り出すことが可能

Github なり、Source Repositories なり、プロジェクトページに行って\
コミットヒストリーの中から、適用なコミット ID をコピペしておく

```bash
# 指定したコミットに状態を戻す
git reset --hard COMMIT

# 元に戻る
git pull origin master
```

## 02. 遡って（さかのぼって）ブランチ分けする

例えば、master ブランチだけで開発していて、ちょっと「進みすぎた」とする\
「進みすぎた」というのは、ある機能を開発していて、何個かコミットしてきたけれども\
リリースは時期尚早と感じたときのことである\
こんな時は、遡ってブランチ分けしておけばいい

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
# => [dev 6459128]
# => Date: Fri Mar 27 11:00:00 2020 +0900
# => 3 files changed, 23 insertions(+), 18 deletions(-)

git cherry-pick 682f226
# => [dev adc0fc2]
# =>  Date: Fri Mar 27 13:00:00 2020 +0900
# =>  1 file changed, 1 insertion(+), 5 deletions(-)

git cherry-pick 88ab868
# => [dev 7f7e687]
# =>  Date: Fri Mar 27 14:00:00 2020 +0900
# =>  1 file changed, 1 insertion(+), 1 deletion(-)

# サーバーにdevブランチをプッシュする
git push origin dev
# => Enumerating objects: 19, done.
# => Counting objects: 100% (19/19), done.
# => Delta compression using up to 4 threads
# => Compressing objects: 100% (14/14), done.
# => Writing objects: 100% (14/14), 1.43 KiB | 209.00 KiB/s, done.
# => Total 14 (delta 11), reused 0 (delta 0)
# => remote: Resolving deltas: 100% (11/11)
# => To https://source.developers.google.com/p/your-project/r/your-repository
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

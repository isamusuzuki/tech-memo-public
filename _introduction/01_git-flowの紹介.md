# git-flow の紹介

作成日 2019/04/16

## 01. git-flow チートシートを読む

[git\-flow cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/index.ja_JP.html)

> git-flow は git の拡張であり、高度なリポジトリ操作を提供する
>
> Git flow はマージすることをベースとして考えるソリューションで、リベースは行わない

## 02. Windows (Cygwin)にインストールする

wget と util-linux が必要

```bash
wget -q -O - --no-check-certificate https://raw.github.com/petervanderdoes/gitflow-avh/develop/contrib/gitflow-installer.sh install stable | bash
```

## 03. Getting Started（初期化）

```bash
# 通常のgitリポジトリの配下に移動した後、git-flow化する
git flow init
#=> 対話形式でいくつかの質問に答える、デフォルト値を推奨
```

## 04. Features（通常の開発）

```bash
# developブランチから開発を開始
git flow feature start MYFEATURE

# 開発を終了してdevelopにマージする
git flow feature finish MYFEATURE

# リモートサーバにプッシュする
git flow feature publish MYFEATURE

# 他の人の修正分をローカルに取り込む
git flow feature pull MYFEATURE
```

## 05. Make a release（リリース準備）

```bash
# developブランチからreleaseブランチを作成する
git flow release start RELEASE [BASE]
#=> BASEは指定しないとdevelopブランチのHEAD

# releaseブランチに修正をプッシュする
git flow release publish RELEASE

# リリースの準備完了
git flow release finish RELEASE
#=> releaseブランチをmasterにマージする
#=> masterブランチにリリース用のタグをつける
#=> developブランチにreleaseブランチをマージする
#-> releaseブランチが削除される
```

## 06. Hotfixes （ホットフィックス）

```bash
# masterブランチのタグから緊急対応のブランチを作成
git flow hotfix start VERSION [BASENAME]

git flow hotfix finish VERSION
#=> developとmasterのブランチをマージする
```

---
tags: ubuntu
---

# find, xargコマンド

作成日 2019/11/5

## find コマンドの基本

### ファイルを検索する

```bash=
find . -name "*.csv"
```

findコマンドはスタートディレクトリを必要とする \
下位ディレクトリを再帰的に検査する \
出力したファイルの詳細を表示させるときは`-ls`オプションをつける

### ディレクトリを検索する

```bash=
find / -name order_completed -type d
```

`/usr/local/abcd/staging/`ディレクトリ下にある、\
2019年7月1日以降に更新されたphpファイルを探してリスト表示する

```bash=
find /usr/local/abcd/staging/ -type f -name "*.php" -newermt "20190701" -ls
```

### 検索したディレクトリまたはファイルに対してコマンドを実行する

```bash=
find /var/www/html/mcs1/app/storage -type d -exec sudo chmod 2777 {} +
find /var/www/html/mcs1/app/storage -type f -exec sudo chmod 0777 {} +
```

オプション解説

- `-type d` ... ディレクトリを判別
- `-type f` ... 通常ファイルを判別
- `-type l` ... シンボリックリンクを判別
- `-exec command` ... 検索後コマンドを実行。このとき`{} +`を使うことで、検索結果をコマンドに引き渡している


## findコマンドにある複雑なオプション

[findコマンドの\-pruneオプションのススメ \| roshi\.tv::blog](https://www.roshi.tv/2011/02/find-prune.html)

- `-prune`オプション ... その前に指定された条件に合致するディレクトリは降りずに検索しない
- `-o`オプション ... 左の条件が真なら右の条件を実行する
- `-print`オプション ... 条件に合致したものを出力する（-pruneを使ったら、-oとともに指定する必要あり） 

```bash=
find .

# mmmディレクトリ以下をヒットさせない
find . -name mmm -prune -o -print

# mmmディレクトリ以下とディレクトリをヒットさせない
find . -name mmm -prune -o -type f -print

# bbbディレクトリ以下とyyyディレクトリ以下とディレクトリをヒットさせない
find . \(-name bbb -o -name yyy \) -prune -o type f -print
```

## xargsコマンド

[【 xargs 】コマンド――コマンドラインを作成して実行する：Linux基本コマンドTips（176） \- ＠IT](https://www.atmarkit.co.jp/ait/articles/1801/19/news014.html)


```bash=
# grepコマンドの実行結果を引数にして、headコマンドを実行
grep -l bash /etc/* 2> /dev/null | xargs head -3
```









# ファイル操作コマンド

作成日 2020/02/03、更新日 2020/04/27

## 01. cp コマンド

ファイルをコピーする

```bash
# ファイルは捨てずに、中身を空にする
cp /dev/null debug.log

# ファイルの属性を保持しつつ、コピーする
sudo cp -p file1 file2
```

## 02. ln コマンド

シンボリックリンクを作成する

構文：`ln -s <オリジナルのフルパス> <リンクのフルパス>`

```bash
ln -s /opt/remi/php70/root/usr/bin/php /usr/bin/php
```

## 03. chmod コマンド

ファイルの属性を変更する

### 4 桁目の意味

3 種類の基本パーミッションだけでは設定できない特殊な権限設定を行う

- 4 ... setuid（実行ファイルのオーナーが実行したことになる）
- 2 ... setgid（実行ファイルのグループが継承される）
- 1 ... sticky bit（ファイルをつくった人しか削除できない）

`chmod 2775 /var/www/application` => application ディレクトリ以下に\
作成されるファイル・ディレクトリはすべて、application と同じグループ属性になる

### ファイルやディレクトリのパーミッションを一括で変更する

悪い例

```bash
chmod -R 755 /path/to/dir
```

サブディレクトリのみならず、ファイルのパーミッションまで 755 になる \
それではと 644 に設定すると、ディレクトリも 644 になる

確かな例

```bash
find /path/to/dir -type d -exec chmod 755 {} +
find /path/to/dir -type f -exec chmod 644 {} +
```

ディレクトリは 755 に変換し、ファイルは 644 に変換する

ベストな例

```bash
chmod -R a=rX,u+w /path/to/dir
```

全員に`r`を与え、ディレクトリのみ`x`を与え、\
所有者には追加で`w`を与える\
`-R`はディレクトリ内も設定対象とするオプション

## 04. xargs コマンド

```bash
# grepコマンドの実行結果を引数にして、headコマンドを実行
grep -l bash /etc/* 2> /dev/null | xargs head -3
```

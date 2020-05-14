# バックアップコマンド

作成日 2020/02/03

## 01. rsync コマンド

使用例

```bash
# file sync
rsync -dgptv --delete ~/verif1/application/* /var/www/application/
rsync -dgptv --delete ~/verif1/application/public/* /var/www/application/public/
# directory sync
rsync -rgptv --delete ~/verif1/application/public/audio/ /var/www/application/public/audio
rsync -rgptv --delete ~/verif1/application/public/img/ /var/www/application/public/img
rsync -rgptv --delete ~/verif1/application/public/js/ /var/www/application/public/js
rsync -rgptv --delete ~/verif1/application/src/ /var/www/application/src
rsync -rgptv --delete ~/verif1/application/templates/ /var/www/application/templates
```

オプションの意味

- `-d` ... ディレクトリを再帰しない
- `-r` ... ディレクトリを再帰的に処理する
- `-g` ... 所有グループをそのまま保持する
- `-p` ... パーミッションを保持する
- `-t` ... タイムスタンプを保持する
- `-v` ... 動作内容を表示する
- `--delete` ... 同期元にないファイルを同期先から削除する

その他のオプション

- `-a` ... アーカイブモード、元のパーミッションやグループを保持したまま同期する `-rlptgoD`と同じ
- `-u` ... 追加されたファイルだけをコピーする
- `-z` ... データを圧縮する
- `--existing` ... 更新されたファイルだけをコピーし、追加されたファイルは無視する

## 02. tar コマンド

```bash
tar -cvf linux_notes.tar notes*.txt
# オプション (c)reate (v)erbose (f)ile

tar -xvf linux_notes.tar
# オプション e(x)tract (v)erbose (f)ile

# designサーバーの/mnt/efg/goodsディレクトリをバックアップする
cd /mnt/efg
tar -cvf mnt-efg-goods.tar goods/*
ls -l --block-size=M
#=> 59M mnt-efg-goods.tar

# 他のサーバーで/mnt/efg/goodsディレクトリをレストアする
sudo mkdir -p /mnt/efg
cd /mnt/efg
sudo tar -xvf /var/www/copy/mnt-efg-goods.tar
```

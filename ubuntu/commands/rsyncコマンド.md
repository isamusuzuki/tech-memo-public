# rsync コマンド

作成日 2020/02/03

## rsync コマンドの使用例

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

## オプションの意味

- `-d` ... ディレクトリを再帰しない
- `-r` ... ディレクトリを再帰的に処理する
- `-g` ... 所有グループをそのまま保持する
- `-p` ... パーミッションを保持する
- `-t` ... タイムスタンプを保持する
- `-v` ... 動作内容を表示する
- `--delete` ... 同期元にないファイルを同期先から削除する

## その他のオプション

- `-a` ... アーカイブモード、元のパーミッションやグループを保持したまま同期する `-rlptgoD`と同じ
- `-u` ... 追加されたファイルだけをコピーする
- `-z` ... データを圧縮する
- `--existing` ... 更新されたファイルだけをコピーし、追加されたファイルは無視する

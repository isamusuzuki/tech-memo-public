# ZIP ファイル

作成日 2020/08/26

## 公式ドキュメントを読む

[zipfile \-\-\- ZIP アーカイブの処理 — Python 3\.8\.5 ドキュメント](https://docs.python.org/ja/3/library/zipfile.html)

> ZIP は一般によく知られているアーカイブ (書庫化) および圧縮の標準ファイルフォーマットです。このモジュールでは ZIP 形式のファイルの作成、読み書き、追記、書庫内のファイル一覧の作成を行うためのツールを提供します。

## 解説記事を読む

[Python で ZIP ファイルを圧縮・解凍する zipfile \| note\.nkmk\.me](https://note.nkmk.me/python-zipfile/)

- ZipFile オブジェクトを作成し、write()メソッドで圧縮したいファイルを追加していく
- ZIP ファイルを新規作成する場合は、ZipFile オブジェクトのコンストラクタの第一引数 file に作成する ZIP ファイルのパスを指定し、第二引数 mode を'w'とする
- さらに、第三引数 compression で圧縮方式を指定できる
- write()メソッドでは、第一引数 filename のファイルを第二引数 arcname という名前で ZIP ファイルに書き込む。arcname は省略すると filename がそのまま使われる。arcname にディレクトリ構造を指定することも可能

```python
import zipfile

with zipfile.ZipFile('data/temp/new_comp.zip', 'w',
    compression=zipfile.ZIP_DEFLATED) as new_zip:
    new_zip.write(
        'data/temp/test1.txt', arcname='test1.txt')
    new_zip.write(
        'data/temp/test2.txt', arcname='zipdir/test2.txt')
    new_zip.write(
        'data/temp/test3.txt', arcname='zipdir/sub_dir/test3.txt')
```

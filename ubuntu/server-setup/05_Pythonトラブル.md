# Python トラブル

作成日 2020/02/26

## 01. 問題発生

Windows では問題なく動いていた Python スクリプトを Ubuntu サーバーに持っていったら、\
ModuleNotFoundError が発生した。自分が作成したモジュールが見つからないと言う

## 02. sys.path を確認する

Python のインタラクティブシェルを動かす

```bash
$ python3
>>> import sys
>>> sys.path
['', '/usr/lib/python3.6', '/usr/lib/python3.6/lib-dynload', '/usr/local/lib/python3.6/dist-packages', 'usr/local/lib/python3/dis-packages']
```

=> ここに自分のプロジェクトのルートフォルダがないので、追加する

=> PYTHONPATH 環境変数に追加すると、自動的に sys.path に追加される

## 03. 解決策

`.bashrc`の最後尾に以下追加しておく\
ログインしたときに自動で実行される

```bash
export PYTHONPATH='/home/user/avocado:$PYTHONPATH'
```

# Python トラブルシューティング

作成日 2020/02/26、更新日 2020/05/18

## 01. ModuleNotFoundError 発生

自分が作成したモジュールが見つからない

### sys.path を確認する

Python のインタラクティブシェルを動かす

```bash
$ python3
>>> import sys
>>> sys.path
```

ここに自分のプロジェクトのルートフォルダがない

PYTHONPATH 環境変数に追加すると、自動的に sys.path に追加される

### 解決策

`.bashrc`の最後尾に以下追加しておく\
ログインしたときに自動で実行される

```bash
export PYTHONPATH="/home/user/PROJECT-NAME:$PYTHONPATH"
```

## 02. bdist_wheel コマンドがない問題

プロジェクトの中で、モジュールをインストールしたときに\
`error: invalid command 'bdist_wheel'` と赤字で表示される

### わかってきたこと

グローバル環境には、wheel モジュールがある

```bash
pip3 list
# => ...
# => wheel 0.34.2
```

プロジェクトの中の仮想環境には、wheel がない

```bash
pip list
# => pip           20.0.2
# => pkg-resources 0.0.0
# => setuptools    44.0.0
```

### 解決方法

requirements.txt を使って一括インストールする前に、\
wheel をインストールしておく

```bash
pip install wheel
pip install -r requirements.txt -c constraints.txt
```

# Ubuntu に Python 環境を構築する

作成日 2020/02/26

## 01. Python 環境を整える

```bash
python --version
# => なし

Python3 --version
# => Python 3.6.9

pip3 --version
# => なし

sudo apt install -y python3-pip python3-venv
```

## 02. 自分のプロジェクトを sys.path に登録する

### 問題発生

Windowsでは問題なく動いていたPythonスクリプトをUbuntuに持っていったら、\
ModuleNotFoundErrorが発生した。自分が作成したモジュールが見つからないと言う

### 確認しておくこと

Pythonのインタラクティブシェルを動かす

```bash
$ python3
>>> import sys
>>> sys.path
['', '/usr/lib/python3.6', '/usr/lib/python3.6/lib-dynload', '/usr/local/lib/python3.6/dist-packages', 'usr/local/lib/python3/dis-packages']
```

=> ここに自分のプロジェクトのルートフォルダがないので、追加する\
=> PYTHONPATH環境変数に追加すると、自動的に sys.path に追加される

### 解決策

```bash
export PYTHONPATH='/home/user/avocado:$PYTHONPATH'
```

`.bashrc`の最後尾に追加しておくと、ログインしたときに自動で実行される

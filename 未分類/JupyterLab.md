---
tags: python
---

# JupyterLab

作成日 2019/10/06

## 01. JupyterLabとは

[JupyterLab のすゝめ \- Qiita](https://qiita.com/kirikei/items/a1639954ce5ccaf7ac3c)

> JupyterLabとは，データ分析者に愛されているJupyter notebookの後継機であるWebアプリケーション的IDEのことです。ベータ版が今年リリースされました。Jupyter notebookの開発は一旦終了し，JupyterLabに移行すると公式アナウンスされています。

### 公式ドキュメントを読む

[JupyterLab Documentation](https://jupyterlab.readthedocs.io/en/stable/)

Getting Started

- [Overview](https://jupyterlab.readthedocs.io/en/stable/getting_started/overview.html)
- [Installation](https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html)
- [Starting JupyterLab](https://jupyterlab.readthedocs.io/en/stable/getting_started/starting.html)

User Guide

- [The JupyterLab Interface](https://jupyterlab.readthedocs.io/en/stable/user/interface.html)

## 02. JupyterLab を使えるようにする

```bash=
# pip でインストールする
pip3 install jupyterlab
#=> 長い時間がかかる
```

### 【オプション】トラブルシューティング

ドキュメントが言うとおりに`jupyter-lab`を実行しても、何も起こらなかった場合\
`which jupyter-lab`を実行しても、返事がない場合

```bash=
# インストール先を知る
pip3 show jupyterlab
#=> Location: /home/username/.local/lib/python3.6/site-packages
#=> ここにも、jupyter-labコマンドはなかった
#=> 付近を探したら、/home/username/.local/binにあった

# 実行ファイルのありかを確認する
ls ~/.local/bin
#=> jupyter, jupyter-lab, jupyter-notebook...
```

PATH にディレクトリを追加する

```bash
vi ~/.bashrc
#=> 最終行に "PATH=${PATH}:~/.local/bin" を追加する

# ちゃんと追加されたか確認する
source .bashrc
env | grep PATH

which jupyter-lab
#=> /home/username/.local/bin/jupyter-lab
```

### JupyterLabを起動する

```bash
jupyter-lab
#=> 自動的にブラウザが起動し、http://localhost:8888/lab を開く

# 外部からも使えるようにする
jupyter-lab --ip=0.0.0.0 --no-browser
#=> http://<IP Address>:8888/lab を開く
```

## 03. JupyterLabを使ってみる

### pipでインストールするとやたら遅い件

pipはビルドが始まってしまって、ものすごく時間がかかることが多い

aptでビルド済みのパッケージをインストールしたほうが断然早い

```bash=
# パッケージがあるか調べる
apt search python3-pandas

# 見つかったら、インストールする
sudo apt install python3-pandas
```

その他、apt searchで見つかったパッケージ

- python3-numpy
- python3-imaging
- python3-matlotlib

### 最初のサンプルコード

```python=
import pandas
df = pandas.read_csv('iris.csv')
df.head(20)
```

iris.csv のありか => `/usr/lib/python3/dist-packages/pandas/tests/data`

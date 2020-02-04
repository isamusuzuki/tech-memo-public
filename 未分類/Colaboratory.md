---
tags: gcp
---

# Colaboratory

作成日 2019/08/04

## Colaboratoryとは

設定不要のJupyterノートブック環境。作成したノートブックは、Googleドライブに保存される。Python 2.7とPython 3.6に対応している

公式ガイドも、Colaboratoryの1ページとなっている

[Colaboratory へようこそ \- Colaboratory](https://colab.research.google.com/notebooks/welcome.ipynb?hl=ja)

## 02. 再挑戦した場合の新規ノートブック作成方法

すでにGoogle DriveとColaboratoryの連携は終わっているが、\
連携させたフォルダを削除してしまったところから、再挑戦したいと思った

- 先に実行環境に行く
- オレンジ色のダイアログが登場する
- 右下にある「PYTHON3の新しいノートブック」をクリック
- 新しいノートブックが開く
- メニュー ＞ ファイル ＞ 保存
- Google DriveにColab Notebooksフォルダができていてその中に`Untitled0.ipynb`が保存されていた
- ノートブックを開くときは、ファイルを右クリック ＞ アプリで開く ＞ Google Colaboratory

### 実際に動かしてみたシステムアリアス

よく使うコマンドへのショートカットを、システムアリアスという

```bash
!os /etc/os-release
#=> NAME="Ubuntu"
#=> VERSION="18.04.2 LTS (Bionic Beaver)"

!python --version
#=> Pytrhon 3.6.8

!pip list
# 主要なライブラリがずらりとインストールされていた
```

実行環境 => [https://colab.research.google.com/](https://colab.research.google.com/)

## 03. 紹介記事1を読む

[Google Colab の知っておくべき使い方](https://www.codexa.net/how-to-use-google-colaboratory/)

### Google Colaboratoryとは

Google Colab（略式した呼称）とは、教育や研究機関へ機械学習の普及を目的としたGoogleの研究プロジェクトの一つで、Jupyter Notebookを必要最低限の労力とコストで利用でき、ブラウザとインターネットがあれば今すぐにでも機械学習のプロジェクトを進めることが可能なサービスです。

### Google Colabのメリット

- 環境構築がほぼ不要
- チーム内での共有が簡単
- GPU を含めて無料で利用が可能

### Google Colabを使う

- Googleアカウントにログインする
- Google Drive で新規フォルダを作成する
- そのフォルダと Google Colab を連携させる
- フォルダ名のドロップダウンをクリックして「アプリで開く」＞「アプリを追加」
- ポップアップの検索窓で「colaboratory」を調べる
- 「＋接続」ボタンをクリック
- 本枠を右クリック ＞ その他 ＞ Colaboratory で新規ファイルを作成する
- 新規作成したノートブックの名前を変更する

### Google Colabのセルの基本操作

- ノートブックはセル単位でコーディングを行う
- セルは2種類ある。コードセルとテキストセル

### コードセルの基本構造

- 左にセルの実行ボタン、`Shift + Enter`と同じ
- 右上にその他の操作がまとまっている
- セルごとにコメントの挿入が可能で、共同作業に便利

### Google ColabでGPUを使う

- ノートブックのデフォルトの設定は、GPUの使用がオフになっている
- メニューバー ＞ 編集 ＞ ノートブックの設定
- ハードウェアアクセラレータをNoneからGPUに変更
- 保存ボタン

```python=
# TensorFlow経由でデバイス設定の確認を行う
from tensorflow.python.Client import device_lib
device_lib.list_local_devices()
```

### Google colabでライブラリを追加する

機械学習に必要なライブラリは、あらかじめインストールされている

- numpy
- matplolib
- pandas
- seaborn
- Tensorflow

```bash
# よく使うコマンドへのショートカットを、システムアリアスという
!pip list
```

## 04. 紹介記事2を読む

[【秒速で無料GPUを使う】深層学習実践Tips on Colaboratory \- Qiita](https://qiita.com/tomo_makes/items/b3c60b10f7b25a0a5935)

Colaboratoryを立ち上げると、時間限定で下記の環境が割り当てられる。

- `n1-highmem-2`インスタンス
- Ubuntu 18.04
- 2vCPU@2.2GHz
- 13GB RAM
- 40GB ~ 360GB Storage
- NVIDIA Tesla K80
- アイドル状態が90分続くと停止する
- 12時間経つと強制的に停止する

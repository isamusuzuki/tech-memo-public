# Colaboratory

作成日 2019/08/04、更新日 2021/03/26

## 01. Colaboratory とは

- 設定不要の Jupyter ノートブック環境
- 作成したノートブックは、Google ドライブに保存される
- Python 3.7 に対応している
- 下の公式ガイドも、Colaboratory の 1 ページとなっている

公式ガイド => [Colaboratory へようこそ \- Colaboratory](https://colab.research.google.com/notebooks/welcome.ipynb?hl=ja)

## 03. 紹介記事を読む 1

紹介記事 => [Google Colab の知っておくべき使い方](https://www.codexa.net/how-to-use-google-colaboratory/)

### Google Colaboratory とは

Google Colab（略式した呼称）とは、教育や研究機関へ機械学習の普及を目的とした Google の研究プロジェクトの一つで、Jupyter Notebook を必要最低限の労力とコストで利用でき、ブラウザとインターネットがあれば今すぐにでも機械学習のプロジェクトを進めることが可能なサービスです。

### Google Colab のメリット

- 環境構築がほぼ不要
- チーム内での共有が簡単
- GPU を含めて無料で利用が可能

### Google Colab を使う

- Google アカウントにログインする
- Google Drive で新規フォルダを作成する
- そのフォルダと Google Colab を連携させる
- フォルダ名のドロップダウンをクリックして「アプリで開く」＞「アプリを追加」
- ポップアップの検索窓で「colaboratory」を調べる
- 「＋接続」ボタンをクリック
- 本枠を右クリック ＞ その他 ＞ Colaboratory で新規ファイルを作成する
- 新規作成したノートブックの名前を変更する

### Google Colab のセルの基本操作

- ノートブックはセル単位でコーディングを行う
- セルは 2 種類ある。コードセルとテキストセル

### コードセルの基本構造

- 左にセルの実行ボタン、`Shift + Enter`と同じ
- 右上にその他の操作がまとまっている
- セルごとにコメントの挿入が可能で、共同作業に便利

### Google Colab で GPU を使う

- ノートブックのデフォルトの設定は、GPU の使用がオフになっている
- メニューバー ＞ 編集 ＞ ノートブックの設定
- ハードウェアアクセラレータを None から GPU に変更
- 保存ボタン

```python
# TensorFlow経由でデバイス設定の確認を行う
from tensorflow.python.Client import device_lib
device_lib.list_local_devices()
```

### Google colab でライブラリを追加する

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

## 04. 紹介記事を読む 2

紹介記事 => [【秒速で無料 GPU を使う】深層学習実践 Tips on Colaboratory \- Qiita](https://qiita.com/tomo_makes/items/b3c60b10f7b25a0a5935)

Colaboratory を立ち上げると、時間限定で下記の環境が割り当てられる。

- `n1-highmem-2`インスタンス
- Ubuntu 18.04
- 2vCPU@2.2GHz
- 13GB RAM
- 40GB ~ 360GB Storage
- NVIDIA Tesla K80
- アイドル状態が 90 分続くと停止する
- 12 時間経つと強制的に停止する

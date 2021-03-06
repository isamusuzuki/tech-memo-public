# Jupyter

作成日 2021/03/06

マイクロソフト 提供。VSCode 内で Jupyter Notebook を操作できるようにする

[Jupyter \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter)

## 01. Jupyter 拡張機能の使い方

- `Ctrl+Shift+p` で、コマンドパレットを出す
- `Jupyter: Create New Blank Notebook` を実行する
- `Untitled-1.ipynb` が登場する
- 最初のセルに、何か Python コードを書き、それを実行する
- ノートブック内のメニューにある「Save Notebook」コマンドをクリックする
- 保存するフォルダ名・ファイル名を決めて、保存する
- 2 回目以降は、左枠のエクスプローラーで保存したファイルを選ぶ

### 実行中のディレクトリについて

- `Untitled-1.ipynb` の状態では、カレントディレクトリは、プロジェクトのトップ
- いったんファイルを保存した後は、カレントディレクトリは、ファイルがあるところ

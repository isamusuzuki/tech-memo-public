# Marp でスライドを作成する

作成日 2019/05/24

## 01. Marp とは

Markdown でプレゼンテーションのスライドを作成するシステム

Marp = MarkMarkdown Presentation ecosystem

公式サイト => [https://github.com/marp-team](https://github.com/marp-team)

- Marp ... entrance page
- Marpit ... スキニーなフレームワーク
- Marp Core ... コンバーターのコア部分
- Marp CLI ... CLI インターフェイス

### 基本ルール

水平線`---`でページを分割する

## 02. vscode の拡張機能を使いこなす

デスクトップ版もあるが、もう開発されていないので、vscode の拡張機能を使う => [https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode)

Marp プレビューは、以下のフロントマッターがあるときだけ有効になる

```text
---
marp: true
title: 会社概要
theme: [default, uncover]
paginate: true
---
```

Marp プレビューを表示するには、コマンドパレットから「Markdown: プレビューを横に表示」を選ぶ

## 03. Marp ファイルを PDF ファイルにする

Marp CLI を使う

公式サイト => [https://github.com/marp-team/marp-cli](https://github.com/marp-team/marp-cli)

yarn でインストールする => `yarn global @marp-team/marp-cli`

```bash
marp --version
#=> @marp-team/marp-cli v0.9.1 (/w bundled @marp-team/marp-core v0.9.0)

marp <file-name.md>
#=> HTMLファイルが出来上がる
#=> 矢印キーで次のスライドに移動する

marp <file-name.md> -o <file-name.pdf>
#=> PDFファイルが出来上がる
#=> 画像が表示されない問題発生
```

### PDF ファイルを作成するときの注意点

- Marp CLI で作成した HTML ファイルには、相対ディレクトリが入ったままなので、他人に渡すときは、使っている画像も一緒に渡さないといけない
- Marp CLI で作成した PDF ファイルには、画像が入っていない。ブロークンイメージになる

### PDF ファイルを作成するときの正しい手順

1. Marp CLI でいったん HTML ファイルに変換する
1. Chrome で HTML ファイルを開く（ちゃんと画像が表示されている）
1. Chrome でプリントして、PDF ファイルを生成する
1. 画像も埋め込まれるし、ファイル名が Front Matter のタイトルになっている

### PDF ファイルでプレゼンするときの注意点

Chrome で PDF ファイルを開いた場合

- F11 でブラウザを全画面にする
- フローティングメニューで、ページを拡大縮小して全画面になるようにガンバる
- `PgDn/PgUp`で、スライドをめくる

=> 横長スライドだと次のスライドが見切れる場合がある。HTML 版より使い勝手が悪くなる

Edge で PDF ファイルを開いた場合

- F11 で全画面にする
- PDF 向けメニューのページ表示で、連続スクロールをオフにする
- 矢印キーで、スライドをめくる

=> 使い勝手よし

## 04. Marp の使い方コツ

### イメージを拡大する

タイトルに設定を入れる。1200px がページに収まる最大値のようだ

```text
![width:1200px](images/ec-flow1.png)
```

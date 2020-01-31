# Marpでスライドを作成する

作成日 2019/05/24

## Marpとは

Markdownでプレゼンテーションのスライドを作成するシステム

Marp = MarkMarkdown Presentation ecosystem

公式サイト => [https://github.com/marp-team](https://github.com/marp-team)

- Marp ... entrance page
- Marpit ... スキニーなフレームワーク
- Marp Core ... コンバーターのコア部分
- Marp CLI ... CLIインターフェイス

### 基本ルール

水平線`---`でページを分割する

## vscodeの拡張機能を使いこなす

デスクトップ版もあるが、もう開発されていないので、vscodeの拡張機能を使う => [https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode)

Marpプレビューは、以下のフロントマッターがあるときだけ有効になる

```text
---
marp: true
title: 会社概要
theme: [default, uncover]
paginate: true
---
```

Marpプレビューを表示するには、コマンドパレットから「Markdown: プレビューを横に表示」を選ぶ

## MarpファイルをPDFファイルにする

Marp CLIを使う

公式サイト => [https://github.com/marp-team/marp-cli](https://github.com/marp-team/marp-cli)

yarnでインストールする => `yarn global @marp-team/marp-cli`

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

### PDFファイルを作成するときの注意点

- Marp CLIで作成したHTMLファイルには、相対ディレクトリが入ったままなので、他人に渡すときは、使っている画像も一緒に渡さないといけない
- Marp CLIで作成したPDFファイルには、画像が入っていない。ブロークンイメージになる

### PDFファイルを作成するときの正しい手順

1. Marp CLIでいったんHTMLファイルに変換する
1. ChromeでHTMLファイルを開く（ちゃんと画像が表示されている）
1. Chromeでプリントして、PDFファイルを生成する
1. 画像も埋め込まれるし、ファイル名がFront Matterのタイトルになっている

### PDFファイルでプレゼンするときの注意点

ChromeでPDFファイルを開いた場合

- F11でブラウザを全画面にする
- フローティングメニューで、ページを拡大縮小して全画面になるようにガンバる
- `PgDn/PgUp`で、スライドをめくる

=> 横長スライドだと次のスライドが見切れる場合がある。HTML版より使い勝手が悪くなる

EdgeでPDFファイルを開いた場合

- F11で全画面にする
- PDF向けメニューのページ表示で、連続スクロールをオフにする
- 矢印キーで、スライドをめくる

=> 使い勝手よし

## Marpの使い方コツ

### イメージを拡大する

タイトルに設定を入れる。1200pxがページに収まる最大値のようだ

```text
![width:1200px](images/ec-flow1.png)
```



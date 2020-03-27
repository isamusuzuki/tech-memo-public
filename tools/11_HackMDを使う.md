# HackMD を使う

作成日 2019/11/29

## 01. HackMD とは

オンライン Markdown エディタのひとつ

公式サイト => [https://hackmd.io/](https://hackmd.io/)

ドキュメント => [https://hackmd.io/c/tutorials/%2Fs%2Ftutorials](https://hackmd.io/c/tutorials/%2Fs%2Ftutorials)

機能紹介（日本語） => [https://hackmd.io/s/features-jp](https://hackmd.io/s/features-jp)

### HackMD の紹介記事を読む

[HackMD って MarkdownEditor が革新的で使いやすい \- Qiita](https://qiita.com/norinity1103/items/85aa990dbe6582b6d701)

> HackMD の最大の強みは、共同編集できて、気軽に始められることだ。\
> MTG の予定をセットしたカレンダーとかに、URL 貼っておけば、メンバーがすぐにアクセスして編集できる。これがわりと楽しい。\
> MTG 後にわざわざ認識合わせとかせずに、リアルタイムで相互レビューができるので、司会進行者のスキル差に関係なく、一定品質のドキュメントが完成できるのだ。

=> 個人用というよりも、共有を前提としたチーム開発で力を発揮するツールだと理解した

[HackMD で Qiita 記事を執筆するときの手順 \- Qiita](https://qiita.com/butada/items/7b5a3b2972f27da7b2a4)

> GitHub アカウントでサインアップする

=> GitHub リポジトリと記事ごとのプル・プッシュができるからだと理解した

## 02. 共有レベルの初期値を理解する

ページ右上、自分のアイコンの左に共有アイコンあり\
初期値は Limited になっている。\
その意味は「Read=Everyone, Write=Signed-in users」で\
情報は、ほぼ駄々洩れであると言わざるを得ない

Note Permission コーナー

| 項目  | 選択肢                                |
| ----- | ------------------------------------- |
| Read  | Owners, Signed-in users, **Everyone** |
| Write | Owners, **Signed-in users**, Everyone |

また、初期値は変更できない。\
原則として、パスワードなど秘匿しなければらない情報を書いてはいけない

## 03. GitHub リポジトリと同期する

1. 空ノートか、もしくは Revisions パネルに行く
1. HackMD アプリが、GitHub アカウントにアクセスすることを認証する
1. 認証は、アカウントレベルとリポジトリレベルの 2 段階ある
1. リポジトリの指定は、「アカウント名/リポジトリ名」と書く
1. ブランチの指定は、「master」と書く
1. ファイルも指定する

ファイルを 1 個づつ同期させないといけないので、割りと面倒くさい\
現在はひとつも同期させていない

### HackMD と GitHub の「改行の違い」とは

GitHub と同期させようとしたときに表示されたメッセージ

> このノートの改行は、GitHub のそれとは違うレンダリングがされている。GitHub へのプッシュ以降も、見た目が同じようにレンダリングされるためには、このノートの改行のレンダリングルールを変更する必要がある\
> HackMD は、書きやすさのために、改行（ひとつのエンター）をハードラインブレーク（段落分け）としてレンダリングする。一方、GitHub は、CommonMark 標準に準拠している。レンダリングルールがこのままだと、このノートは、2 つのプラットフォームで、違う見た目になってしまう

HackMD は、CommonMark Spec に準拠しているが、1 箇所だけ違いがある

- Hard line break ... エンターキーを叩くと改行（HackMD）
- Soft line break ... エンターキーは空白として扱う（GitHub）

Soft Line break で、改行したい場合は、2 つの方法がある

- 空白 2 個を入れてから改行する
- 行末にバックスラッシュを入れる

## 04. YAML フロントマッターを使いこなす

```yaml
title: meta title
description: meta description
image: https://hackmd.io/screenshot.png
tags: features, cool, updated
robots: noindex, nofollow
lang: ja-jp
dir: ltr
breaks: false
GA: UA-1234567-8
disqus: hackmd
slideOptions:
  transition: fade
  theme: white
```

## 05. Code ブロックを使いこなす

- 言語名の後ろにイコール(=)をつけると行番号が登場する
- text という言語名はない。代わりに、言語名を指定しないという方法をとる
- 言語名を指定せず、イコール(=)だけという使い方も可能

## 06. モードを切り替える

- 編集モード ... `Ctrl + Alt + E`
- 分割モード ... `Ctrl + Alt + B`
- 表示モード ... `Ctrl + Alt + V`

## 07. ブックを生成する

[How to create a book \- HackMD](https://hackmd.io/c/tutorials/%2Fs%2Fhow-to-create-book)

1. 新しいノートを作成する
2. 最初の行に、ブックタイトルを書き、その下にイコール(=)を 3 つ書く
3. ブックでも YAML フロントマッターを使用できる
4. 1 行空けた後、セクションタイトルを書き、その下にマイナス(-)を 3 つ書く
5. 1 行空けた後、ノートへのリンクを順番なしのリストとして書く
6. ノートへのリンクは、一文字書いたところで、サジェスチョンが登場するので、その中から選択する
7. 右上の共有ボタンをクリックする。sharing ポップアップが登場する
8. View Mode と表示されているドロップダウンリストを Book Mode に変更する
9. 覚えやすい URL を入力する
10. Publish ボタンをクリックする

## 08. スライドを作成する

[Slide example \- HackMD](https://hackmd.io/slide-example)

1. 右上の共有メニューをクリックする
2. モードを Slide Mode に切り替えて、Preview ボタンをクリックする

[reveal.js](https://github.com/hakimel/reveal.js/)を使っている\
細かい仕様はこちらを参照する

YAML フロントマッターで指定することもできる

```yaml
slideOptions:
  transition: fade
  theme: white
```

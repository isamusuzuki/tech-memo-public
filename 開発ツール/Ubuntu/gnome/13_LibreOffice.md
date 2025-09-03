# LibreOffice

作成日 2020/06/19

## 01. LibreOffice とは

オープンソースのオフィススイート

- ワードプロセッサの Writer
- 表計算アプリケーションの Calc
- プレゼンテーションエンジンの Impress
- ドローイングおよびフローチャートのための Draw
- データベースおよびそのフロントエンドとなる Base
- 数式を編集するための Math

最新版 => 6.4.4

公式トップ => [ホーム \| LibreOffice \- オフィススイートのルネサンス](https://ja.libreoffice.org/)

英語ドキュメント => [English documentation \| LibreOffice Documentation \- Your documentation for LibreOffice](https://documentation.libreoffice.org/en/english-documentation/)

日本語ドキュメント => [ドキュメント \| LibreOffice Documentation \- Your documentation for LibreOffice](https://documentation.libreoffice.org/ja/documentation-in-japanese/)

## 02. LibreOffice Calc を使う

### シグマ (SUM) ボタンが見つからない

ない模様。代わりに、右端にある Functions ボタンを押すと、最近使った関数の一覧が登場する

### シート上に配置されたボタンを編集するには

ボタンを右クリックしても、ショートカットメニューは登場せず、何も起こらない

View メニュー ＞ Toolsbars ＞ Form Controls ＞ Form Controls ダイアログが登場\
Design Mode アイコン（定規とペン）をクリック ＞ 目的のボタンをクリック\
＞ ボタンに 8 つのコントロールが登場\
右クリック ＞ ショートカットメニュー ＞ Control Properties ＞ Properties ダイアログが登場\
Events タブ ＞ Execute action テキストボックスに、割り振られているメソッド名がある

## 03. LibreOffice Base を使う

### VBA マクロが使える

[LibreOffice で VBA を使う方法。1 行おまじないを書くだけ！ \| 非 IT 企業に勤める中年サラリーマンの IT 日記](http://pineplanter.moo.jp/non-it-salaryman/2017/03/02/vba-on-libreoffice/)

### マクロ入りエクセルファイルを開けるようにする

Tools メニュー ＞ Options... コマンド ＞ Options ウィンドウが開く\
左枠 ＞ LibreOffice ＞ Security\
右枠 ＞ Macro Security ＞ Macro Security... ボタン ＞ Macro Security ウィンドウが開く\
Security Level タブ ＞ High を Medium に変更 ＞ OK ボタン 2 回

### マクロを表示する

Tools メニュー ＞ Macros ＞ Edit Macros ＞ LibreOffice Basic が開く

### Application.GetOpenFilename がエラーになる

LibreOffice ではサポートされていない関数

[118246 – FILEOPEN XLSX Basic code with “GetOpenFilename” method does not work](https://bugs.documentfoundation.org/show_bug.cgi?id=118246)

> Actual results: The file dialog is opened in Microsoft Office Excel, but If We use this “GetOpenFilename” method in LibreOffice Calc, we get an error message. If we use the LibreOffice module CreateUnoService("com.sun.star.ui.dialogs.FilePicker") we don’t gt an error message.

### 参考記事を読む

[VBAtoLibreOffice\.pdf](http://freesol.web.fc2.com/application/VBAtoLibreOffice.pdf)

[ExceltoCalc\.pdf](https://www.ja-fukuoka.or.jp/upload/user/ExceltoCalc.pdf)

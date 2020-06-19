# LibreOffice Baseを使う

作成日 2020/06/19

## VBAマクロも使える

[LibreOfficeでVBAを使う方法。1行おまじないを書くだけ！ \| 非IT企業に勤める中年サラリーマンのIT日記](http://pineplanter.moo.jp/non-it-salaryman/2017/03/02/vba-on-libreoffice/)

## マクロ入りエクセルファイルを開けるようにする

Tools メニュー ＞ Options... コマンド ＞ Optionsウィンドウが開く\
左枠 ＞ LibreOffice ＞ Security\
右枠 ＞ Macro Security ＞ Macro Security... ボタン ＞ Macro Security ウィンドウが開く\
Security Level タブ ＞ HighをMediumに変更 ＞ OKボタン2回

## マクロを表示する

Tools メニュー ＞ Macros ＞ Edit Macros ＞ LibreOffice Basicが開く

## Application.GetOpenFilenameがエラーになる

LibreOfficeではサポートされていない

[118246 – FILEOPEN XLSX Basic code with “GetOpenFilename” method does not work](https://bugs.documentfoundation.org/show_bug.cgi?id=118246)

> Actual results: The file dialog is opened in Microsoft Office Excel, but If We use this “GetOpenFilename” method in LibreOffice Calc, we get an error message. If we use the LibreOffice module CreateUnoService("com.sun.star.ui.dialogs.FilePicker") we don’t gt an error message.

## 参考記事を読む

[VBAtoLibreOffice\.pdf](http://freesol.web.fc2.com/application/VBAtoLibreOffice.pdf)

[ExceltoCalc\.pdf](https://www.ja-fukuoka.or.jp/upload/user/ExceltoCalc.pdf)

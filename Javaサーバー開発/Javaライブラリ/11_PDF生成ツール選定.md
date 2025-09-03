# PDF 生成ツール選定

作成日 2025/01/20、更新日 2025/01/21

## 候補 1. [Apache PDFBox](https://pdfbox.apache.org/)

- No.1 in PDF Libraries of Maven Repository
- The Apache PDFBox library is an open source Java tool for working with PDF documents.
- 最新バージョン(3.0.x 系) => 3.0.3 at 2024-08-08
- 最新バージョン(2.0.x 系) => 2.0.33 at 2025-01-16

参照サイト => [Java ライブラリ Apache PDFBox で PDF を操作しよう](https://weblabo.oscasierra.net/java-pdfbox-1/)

## 候補 2. [iText](https://itextpdf.com/)

- No.2 & 3 in PDF Libraries of Maven Repository
- iText is dual licensed as AGPL/Commercial software. => `コンセプト/dソフトウェアライセンス/11_AGPL.md`を参照のこと
- 最新バージョン(iText Core/Community) => 9.0.0 at 2024-11-18
- ソースコード => [https://github.com/itext/itext-java](https://github.com/itext/itext-java)
- ナレッジベース => [https://kb.itextpdf.com/](https://kb.itextpdf.com/)

iText おすすめポイント

1. サンプルコードが豊富
1. 日本語フォントを扱える
1. テーブルエレメントを持っている

カラムの幅を指定し、かつテーブルにヘッダー・フッターをとりつけた例 => [cmp_column_width_example.pdf](https://github.com/itext/itext-publications-examples-java/blob/develop/cmpfiles/sandbox/tables/cmp_column_width_example.pdf)

## 候補 3. [OpenPDF](https://librepdf.github.io/OpenPDF/)

- No.4 in PDF Libraries of Maven Repository
- OpenPDF is based on a fork of iText
- OpenPDF is a free Java library for creating and editing PDF files with a LGPL and MPL open source license.
- 最新バージョン => 2.0.3 at 2024-08-07
- ソースコード => [LibrePDF/OpenPDF](https://github.com/LibrePDF/OpenPDF)

## 候補 4. [JasperReports Library](https://community.jaspersoft.com/download-jaspersoft/community-edition/)

参照サイト => [JasperReports で帳票出力してみた](https://dev.classmethod.jp/articles/jasperreports-tutorial/)

> 帳票を処理するための Java のライブラリ
> 独自の XML フォーマットで定義したテンプレートを元に帳票を作成し、PDF や Excel などに出力することができる
> テンプレートとなる XML ファイルは、Jaspoersoft Studio という専用のデザインツールを利用する

- 最新バージョン => 7.0.0 at 2024-06-17
- ソースコード => [TIBCOSoftware/jasperreports](https://github.com/TIBCOSoftware/jasperreports)
- LGPL ライセンス採用 => [jasperreports/LICENSE](https://github.com/TIBCOSoftware/jasperreports/blob/master/LICENSE)
- JasperReports は PDF 作成に OpenPDF 1.3.32 を利用している => [pom-parent.xml](https://github.com/TIBCOSoftware/jasperreports/blob/7.0.0/pom-parent.xml)

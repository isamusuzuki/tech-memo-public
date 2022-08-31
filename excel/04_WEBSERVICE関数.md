# WEBSERVICE 関数

作成日 2022/08/29

## WEBSERVICE 関数と Excel API

解説記事を発見 => [日付、住所、文字列、辞書、翻訳……なんでもござれの「ExcelAPI」がスゴい](https://forest.watch.impress.co.jp/docs/serial/yajiuma/1435224.html)

> 「ExcelAPI」というWebサイトが話題になっていました。「Excel 2013」以降で利用できる「WEBSERVICE」関数を用いて、さまざまなデータを「Excel」に取り込むためのデータ提供を目的としたサイト――なのだそうです。

```text
=WEBSERVICE("http://api.excelapi.org/post/address?zipcode=1000014")
```

=> ブラウザでこのURLを叩いてみたら、最初はいつまでも返事がなく、Bad Gateway が出てくるようになったと思ったら、「東京都千代田区永田町」をテキストで戻すようになった

[郵便番号から住所を取得 | ExcelAPI](https://excelapi.org/docs/post/address/)

## 外部サイトを取り込むための機能を発見

[JSON形式のWebAPIをExcel形式に変換 ★ | ExcelAPI](https://excelapi.org/docs/convert/json2plain/)

> JSON 形式を返す Webサイトの応答データを、Excel 形式(平文、plaintext)に変換して返します。
これにより、様々な WebAPI を WEBSERVICE 関数で利用できます。

```text
=WEBSERVICE("https://api.excelapi.org/convert/json2plain?url="&ENCODEURL($A2)&"&target="&ENCODEURL($B2))
```

=> 複数の値を別のセルに書き込むにはどうする？

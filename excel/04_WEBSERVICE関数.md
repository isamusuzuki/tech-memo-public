# WEBSERVICE 関数

作成日 2022/08/29

## 01. WEBSERVICE 関数と Excel API

解説記事を発見 => [日付、住所、文字列、辞書、翻訳……なんでもござれの「ExcelAPI」がスゴい](https://forest.watch.impress.co.jp/docs/serial/yajiuma/1435224.html)

> 「ExcelAPI」という Web サイトが話題になっていました。「Excel 2013」以降で利用できる「WEBSERVICE」関数を用いて、さまざまなデータを「Excel」に取り込むためのデータ提供を目的としたサイト――なのだそうです。

```text
=WEBSERVICE("http://api.excelapi.org/post/address?zipcode=1000014")
```

=> ブラウザでこの URL を叩いてみたら、最初はいつまでも返事がなく、Bad Gateway が出てくるようになったと思ったら、「東京都千代田区永田町」をテキストで戻すようになった

[郵便番号から住所を取得 | ExcelAPI](https://excelapi.org/docs/post/address/)

## 02. 外部サイトを取り込むための機能を発見

[JSON 形式の WebAPI を Excel 形式に変換 ★ | ExcelAPI](https://excelapi.org/docs/convert/json2plain/)

> JSON 形式を返す Web サイトの応答データを、Excel 形式(平文、plaintext)に変換して返します。
> これにより、様々な WebAPI を WEBSERVICE 関数で利用できます。

```text
=WEBSERVICE("https://api.excelapi.org/convert/json2plain?url="&ENCODEURL($A2)&"&target="&ENCODEURL($B2))
```

### 引数の説明

| key     | value                                                    |
| ------- | -------------------------------------------------------- |
| url     | アクセスする url, JSON 形式の応答を返す必要あり          |
| target  | 応答された JSON のどの部位を返すか指定する               |
| cache   | キャッシュの使用有無, yes or no, デフォルト:yes          |
| timeout | キャッシュの使用期限（秒）, デフォルト: 86400 秒（1 日） |

### target の指定方法

二重の辞書の場合

```json
{
  "id": 1,
  "name": "tanaka",
  "attribute": {
    "gender": "male",
    "phone_number": "xxxxxxxxxxx",
    "birth": "1991/01/01"
  }
}
```

- name => tanaka
- attribute.gender => male

リストの場合

```json
{
  "status": "OK",
  "data": [
    { "id": "01100", "name": "札幌市" },
    { "id": "01101", "name": "中央区" },
    { "id": "01102", "name": "北区" }
  ]
}
```

- data.0 => { id: '011001', name: '札幌市' }
- data.0.name => 札幌市

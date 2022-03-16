# Excel 関数を解説する

作成日 2021/10/27

公式ドキュメント => [Excel 関数 \(アルファベット順\)](https://support.microsoft.com/ja-jp/office/excel-%E9%96%A2%E6%95%B0-%E3%82%A2%E3%83%AB%E3%83%95%E3%82%A1%E3%83%99%E3%83%83%E3%83%88%E9%A0%86-b3944572-255d-4efb-bb96-c6d90033e188)

## CEILING 関数

指定された基準値の倍数のうち、最も近い値に数値を切り上げます

[CEILING 関数](https://support.microsoft.com/ja-jp/office/ceiling-%E9%96%A2%E6%95%B0-0a5cd7c8-0720-4f0a-bd2c-c943e510899f)

書式: `CEILING(数値,基準値)`

例: `=CEILING(442,5)` => 445

5 の倍数の最も近い価格に切り上げる

## INDEX 関数

セル参照または配列から、指定された位置の値を返します

[INDEX 関数](https://support.microsoft.com/ja-jp/office/index-%E9%96%A2%E6%95%B0-a5dcf0dd-996d-40a4-a822-b56b061328bd)

書式: `INDEX(配列, 行番号, [列番号])`

## MATCH 関数

[MATCH 関数](https://support.microsoft.com/ja-jp/office/match-%E9%96%A2%E6%95%B0-e8dffd45-c762-47d6-bf89-533f4a37673a)

範囲 のセルの範囲で指定した項目を検索し、その範囲内の項目の相対的な位置を返します

書式: `MATCH(検査値, 検査範囲, [照合の型])`

範囲 A1:A3 に値 5, 25, 38 が含まれている場合、数式 `=MATCH(25,A1:A3,0)` は 2 を返す

照合の型

- 1 ... 検査値以下の最大の値を検索する
- 0 ... 検査値と等しい最初の値を検索する
- -1 ... 検査値以上の最小の値を検索する

## VLOOKUP 関数

[VLOOKUP 関数](https://support.microsoft.com/ja-jp/office/vlookup-%E9%96%A2%E6%95%B0-0bbc8083-26fe-4963-8ab8-93a18ad188a1)

書式: `VLOOKUP(検索値, 範囲, 列番号, [検索の型])`

検索の型

- 1/TRUE ... 近似一致
- 0/FALSE ... 完全一致

※ 常に一番左の列で、一致する行を探す。これは暗黙の了解となっているので、注意が必要

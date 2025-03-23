# `@ParameterizedTest` アノテーション

作成日 2025/01/31

参照サイト => [JUnit5 の@ParameterizedTest でテストの表示名を整える](https://qiita.com/unmelt/items/11de927a03826821f074)

## name 引数の中で使える変数

- `{index}` ... 繰り返し中の何番目か（1 始まり）
- `{n}` ... n+1 番目の引数
- `{arguments}` ... 引数全部をカンマ区切りで結合

# JavaScript で URL エンコードする

作成日 2025/01/16

参照サイト => [JavaScript における正しい URL エンコードの方法（encodeURI と encodeURIComponent の違い）](https://qiita.com/sa9ra4ma/items/358c936bc481a4c54866)

> 違い
>
> URL 内で特別な意味を持つ「; , / ? : @ & = + $ #」の扱いが違います。
> encodeURI => エンコードされない
> encodeURIComponent => エンコードされる
>
> URL 全体をエンコードする場合は`encodeURI`で、パラメータなどの部分的な文字列をエンコードする場合は`encodeURIComponent`を使用しましょう。

## encodeURIComponent 変換表

| Key | Value |
| :-: | :---: |
| " " |  %20  |
| "#" |  %23  |
| "$" |  %24  |
| "&" |  %26  |
| "+" |  %2B  |
| "," |  %2C  |
| "/" |  %2F  |
| ":" |  %3A  |
| ";" |  %3B  |
| "=" |  %3D  |
| "?" |  %3F  |
| "@" |  %40  |

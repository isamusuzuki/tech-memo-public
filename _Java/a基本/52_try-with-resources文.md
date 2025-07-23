# try-with-resources 文

作成日 2025/01/31

参照サイト => [try-with-resources 文の基本](https://qiita.com/Takmiy/items/a0f65c58b407dbc0ca99)

> try-with-resources 文が利用できるクラスは、AutoCloseable インタフェースおよびそのサブインタフェースである Closeable インタフェースの実装クラスに限られます
>
> 1. 基本的にリソースは自動開放
> 1. 変数のスコープは try 句に限られる
> 1. close 時の例外は基本的には考慮不要

# OutputStream クラス

作成日 2025/01/31

## java.io.OutputStream

公式マニュアル => [クラス OutputStream](https://docs.oracle.com/javase/jp/8/docs/api/java/io/OutputStream.html)

> この抽象クラスは、バイト出力ストリームを表現するすべてのクラスのスーパー・クラスです。出力ストリームは、出力バイトを受け付けて、特定の受け手に送ります。

実装されたインタフェース

- Closeable
- Flushable
- AutoCloseable

直系のサブクラス

- ByteArrayOutputStream
- FileOutputStream
- FilterOutputStream
- ObjectOutputStream
- PipedOutputStream

## 参照サイトを読む

[今更ながら Java の I/O ストリームを整理する](https://zenn.dev/kawakawaryuryu/articles/8924849b88590cda4e22)

> - InputStream, OutputStream, Reader, Writer は抽象クラスです
> - InputStream, OutputStream はバイト単位でデータを扱います
> - Reader, Writer は文字単位でデータを扱います

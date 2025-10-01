# `@SneakyThrows`の使い方

作成日 2024/12/27、更新日 2025/01/23

参照サイト => [Lombok の@NonNull と@SneakyThrows の具体的な使用方法と注意点](https://www.issoh.co.jp/tech/details/3300/#LombokNonNullSneakyThrows)

> `@NonNull`は、メソッドの引数やフィールドが null であってはならないことを示し、null チェックを自動的に行います。
> `@SneakyThrows`は、チェック例外をスローするメソッドを簡略化し、try-catch ブロックを記述せずに例外処理を行うことができます。

```java
// @NonNullを使った引数チェックの自動化
public void setName(@NonNull String name) {
    this.name = name;
}

// @SneakyThrowsを使って例外処理を簡略化する
@SneakyThrows
public void readFile(String path) {
    Files.readAllLines(Paths.get(path));
}
```

# Properties クラス

作成日 2025/02/04

## キーバリュー形式のファイルを読み書きする

参照サイト => [Java の設定ファイル - Properties の使い方](https://java.keicode.com/lang/properties.php)

- 設定情報とアプリケーションの実行ファイルを明確に分ける
- Properties クラスを使って、キー・バリュー形式のファイルの読み込みと保存ができる
- キー・バリューはどちらも String 型

WriteProperties.java

```java
Properties settings = new Properties();
settings.setProperty("country", "USA");
settings.setProperty("lang", "English");

try (FileOutputStream os = new FileOutputStream(filename)) {
    settings.store(os, "First Settings");
} catch (IOException e) {
    e.printStackTrace();
}
```

ReadProperties.java

```java
Properties settings = new Properties();

try (FileInputStream is = new FileInputStream(filename)) {
    settings.load(is);
} catch (IOException e) {
    e.printStackTrace();
}
log.info("lang => {}", settings.getProperty("lang"));
log.info("country => {}", settings.getProperty("country"));
settings.list(System.out);
```

## jar ファイルの外にーバリュー形式のファイルを置く

参照サイト => [プロパティファイルを外に置きたい！](https://iidaapp.hatenablog.com/entry/2014/10/08/173153)

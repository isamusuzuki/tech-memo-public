# DBFlute が生成する SQL クエリーをログで確認する

作成日 2024/12/24、更新日 2024/12/27

`src/main/resource`フォルダに`logback-spring.xml`ファイルを作成する

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE logback>
<configuration>
  <!-- Spring Bootのデフォルト設定を読み込む -->
  <include resource="org/springframework/boot/logging/logback/defaults.xml" />
  <include resource="org/springframework/boot/logging/logback/console-appender.xml" />

  <!-- INFOレベルのログをコンソールに出力させる-->
  <root level="INFO">
    <appender-ref ref="CONSOLE"/>
  </root>

  <!-- DBFluteが生成したSQLクエリーをコンソールに出力させる -->
  <logger name="org.dbflute.system.QLog" level="DEBUG">
    <appender-ref ref="CONSOLE"/>
  </logger>

</configuration>
```

- `org.dbflute.system.QLog`は、クエリログ(表示用 SQL の実行ログ)を出力するクラス
- `org.dbflute.system.XLog`は、実行ステータスログを出力するクラス

参照サイト => [DBFlute ランタイム](https://dbflute.seasar.org/ja/manual/function/ormapper/runtime/index.html)

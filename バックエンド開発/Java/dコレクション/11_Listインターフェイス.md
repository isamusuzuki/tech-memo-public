# Listインターフェイス

作成日 2025/01/17、更新日 2025/03/25

## 1. ListとArrayListの違い

参照サイト => [Java のコレクションクラスまとめ(List と ArrayList の違いも解説)](https://www.sejuku.net/blog/14886)

- List ... インタフェースなのでインスタンス化できない
- ArrayList ... List インタフェースを実装したコレクションクラス
- LinkedList ... ArrayList + 要素がどのように並んでいるか管理

```java
List<Integer> foo = new ArrayList<Integer>();

List<Integer> bar = new LinkedList<Integer>();
```

## 2. `List.of()`は変更不可能なリストを返す

問題発生

```java
List<String> columns = List.of("曜日", "時間", "実習内容", "担当教官", "場所");

// 変更不可能なリストでadd()を実行すると、
// UnsupportedOperationExceptionがスローされる
columns.add("備考");
```

問題解決

```java
List<String> columns = new ArrayList<>();
for (String column : List.of("曜日", "時間", "実習内容", "担当教官", "場所")) {
    columns.add(column);
}

columns.add("備考");
```

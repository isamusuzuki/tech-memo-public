# stream メソッドを使う

作成日 2024/12/27、更新日 2025/03/06

## `stream()`とは

コレクション（List,Set,Map）に対して、様々な処理を行うためのもの

for ループで書くよりも、コードが簡潔になる

## 具体的な使い方

`map(lambda)`を使う

```java
List<Integer> intList = new ArrayList<Intger>(Arrays.asList(1, 2, 3, 4, 5));

List<Integer> ret = intList.stream().map(x -> x ^ 2).toList();
```

`filer(lambda)`を使う

```java
List<Integer> intList = new ArrayList<Intger>(Arrays.asList(1, 2, 3, 4, 5));

List<Integer> ret = intList.stream().filer(x -> x % 2 == 0).toList();
```

`sorted()`を使う

```java
List<Integer> intList = new ArrayList<Intger>(Arrays.asList(1, 2, 3, 4, 5));

List<Integer> ret = intList.stream().sorted(Comparator.reverseOrder).toList();
```

## `.toList()`メソッドの導入

以下は同じ

```java
// Java16よりも前の書き方
List<Integer> ret = intList.stream().map(x -> x ^ 2).collect(Collectors.toList());

// Java16で導入された
List<Integer> ret = intList.stream().map(x -> x ^ 2).toList();
```

# List から配列へ変換

作成日 2025/01/27、更新日 2025/02/26

## 1. toArray メソッドを使ってリストを配列に変換する

参照サイト => [Java】List から配列へ変換](https://zenn.dev/goriki/articles/038-list-to-array)

```java
List<Integer> list = List.of(0, 1, 2);

Integer[] array = list.toArray(new Integer[list.size()]);
```

参照サイト 2 => [【Java 入門】List⇔ 配列の相互変換は”toArray”と”asList”で OK！](https://www.sejuku.net/blog/16155)

```java
List<String> list = new ArrayList<>();
list.add("い");
list.add("ろ");
list.add("は");

String[] array = list.toArray(new String[list.size()]);
```

## 2. リストをプリミティブ型の配列を変換する

### Integer のリストを int の配列に変換する

```java
int[] ar = li.stream().mapToInt(Integer::intValue).toArray();
```

### Float のリストを float の配列に変換する

```java
List<Float> li = List.of(0f, 1f, 2f);

float[] ar = new float[li.size()];
int i = 0;
for (Float number: li) {
    ar[i++] = number;
}
```

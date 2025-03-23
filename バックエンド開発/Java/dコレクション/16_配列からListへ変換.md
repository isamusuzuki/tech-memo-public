# 配列から List への変換

作成日 2025/03/06

## Copilot が教えてくれたコードを少し変更

```java
public List<String> concat(String[] array1, String[] array2) {
    List<String> joinedList = Stream.concat(
        Arrays.stream(array1),
        Arrays.stream(array2)
    ).toList();
    return joinedList;
}
```

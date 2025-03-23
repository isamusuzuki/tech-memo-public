# List と ArrayList の違い

作成日 2025/01/17

参照サイト => [Java のコレクションクラスまとめ(List と ArrayList の違いも解説)](https://www.sejuku.net/blog/14886)

- List ... インタフェースなのでインスタンス化できない
- ArrayList ... List インタフェースを実装したコレクションクラス
- LinkedList ... ArrayList + 要素がどのように並んでいるか管理

```java
List<Integer> foo = new ArrayList<Integer>();

List<Integer> bar = new LinkedList<Integer>();
```

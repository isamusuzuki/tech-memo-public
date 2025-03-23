# Map インタフェース

作成日 2025/01/17、更新日 2025/01/27

公式マニュアル（日本語） => [インタフェース Map<K,V>](https://docs.oracle.com/javase/jp/8/docs/api/java/util/Map.html)

> キーを値にマッピングするオブジェクトです。マップには、同一のキーを複数登録できません。各キーは 1 つの値にしかマッピングできません。

## HashMap クラスを使ってインスタンスを生成する

参照サイト => [【Java】Map の使い方](https://qiita.com/mzmz__02/items/0c3c0a0576fd15d3049a)

```java
import java.util.HashMap;
import java.util.Map;

// キーをInteger、値をStringにする場合
Map<Integer, String> map = new HashMap<>();
```

## Map でよく使用するメソッド

```java
Map<Integer, String> map = new HashMap<>();
// putメソッド
map.put(1, "田中");
map.put(3, "鈴木");
map.put(5, "山田");

// getメソッド
System.out.println(map.get(1));
System.out.println(map.get(3));
System.out.println(map.get(5));
// 田中
// 鈴木
// 山田

// keySetメソッド
for(Integer key:map.keySet()) {
            System.out.println(key);
}
// 1
// 3
// 5

// valuesメソッド
for(String value: map.values()) {
            System.out.println(value);
}
// 田中
// 鈴木
// 山田
```

## キー名でソートする方法

参照サイト => [【Java】Map(HashMap・TreeMap)の key・value でソートする方法](https://www.sejuku.net/blog/16176)

```java
// Mapの宣言
Map<Integer, String> mMap = new HashMap<Integer, String>();

// Mapにデータを格納
mMap.put( 1, "apple");
mMap.put( 2, "orange");
mMap.put( 4, "pineapple");
mMap.put( 5, "strawberry");
mMap.put( 3, "melon");

// キーでソートする
Object[] mapkey = mMap.keySet().toArray();
Arrays.sort(mapkey);

for (Integer nKey : mMap.keySet())
{
    System.out.println(mMap.get(nKey));
}
```

### TreeMap を使えば、キー名でソートする必要なし

[キーに順序を持ったマップ – TreeMap クラス](https://java-code.jp/230)

HashMap ではキーの順番を保証しないのに対して、TreeMap ではキーを自動的にソートし、順序を保証します。 キーの重複を許さない点は、HashMap と同じですし、利用できるメソッドも共通です

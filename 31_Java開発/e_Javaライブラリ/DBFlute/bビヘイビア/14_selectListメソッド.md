# selectList()メソッドを使って複数のデータを取得する

作成日 2024/12/26、更新日 2024/12/27

公式サイト => [selectList(cb)](https://dbflute.seasar.org/ja/manual/function/ormapper/behavior/select/selectlist.html)

## 1. forループでリストからエンティティを取り出すサンプルコード

```java
public class DemoApplication {

    @Autowired
    private TblGakusekiBhv tblGakusekiBhv;

    @GetMapping("/coconut")
    public String coconut() {
        StringBuilder sb = new StringBuilder();
        ListResultBean<TblGakuseki> gakusekiList = tblGakusekiBhv.selectList(cb -> {
            long longId = 140275;
            cb.query().setId_GreaterEqual(longId);
        });
        sb.append(String.format("カウント: %s<br>", gakusekiList.size()));

        for (TblGakuseki gakuseki : gakusekiList) {
            sb.append(String.format("名前: %s<br>", gakuseki.getShimei()));
        }

        return sb.toString();
    }
}
```

- `ListResultBean`は、`java.util.List`の実装クラス
- 検索結果が一件もない場合でも、nullは戻らず、空のリストが戻る

## 2. 外部テーブルを結合させ、forループで外部テーブルの値も取り出すサンプルコード

```java
public class DemoApplication {

    @Autowired
    private TblGakusekiBhv tblGakusekiBhv;

    @GetMapping("/coconut")
    public String coconut() {
        long longId = 140275;
        StringBuilder sb = new StringBuilder();
        ListResultBean<TblGakuseki> gakusekiList = tblGakusekiBhv.selectList(cb -> {
            cb.setupSelect_MstShozoku1();
            cb.query().queryMstShozoku1().innerJoin();
            cb.query().setId_GreaterEqual(longId);
        });
        sb.append(String.format("カウント: %s<br>", gakusekiList.size()));

        for (TblGakuseki gakuseki : gakusekiList) {
            sb.append(String.format("名前: %s, ", gakuseki.getShimei()));

            // やり方A: isPresent()で存在を確認してから、get()で外部テーブルのエンティティを取り出す
            if (gakuseki.getMstShozoku1().isPresent()) {
                MstShozoku1 s1 = gakuseki.getMstShozoku1().get();
                sb.append(String.format("所属: %s<br>", s1.getMeisyou()));
            } else {
                sb.append("所属: 所属名なし<br>");
            }

            // やり方B: OptionalEntityを取得し、さらにifPresent()とorElse()を連続実行する
            gakuseki.getMstShozoku1().ifPresent(s1 -> {
                sb.append(String.format("所属: %s<br>", s1.getMeisyou()));
            }).orElse(() -> {
                sb.append("所属: 所属名なし<br>");
            });
        }

        return sb.toString();
    }
}
```

- ConditionBeanを設定している箇所でも`setupSelect()`を使って外部テーブルを呼び出すし、さらに実際の値を取り出すコードでも、外部テーブルを呼び出す。これがDBFluteの流儀

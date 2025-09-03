# selectEntity()メソッドを使ってデータを1件取得する

作成日 2024/12/24、更新日 2024/12/26

公式サイト => [selectEntity(cb)](https://dbflute.seasar.org/ja/manual/function/ormapper/behavior/select/selectentity.html)

## サンプルコード 1

```java
public class DemoApplication {

    @Autowired
    private TblGakusekiBhv tblGakusekiBhv;

    @GetMapping("/avocado")
    public String avocado() {
        OptionalEntity<TblGakuseki> gakuseki = tblGakusekiBhv.selectEntity(cb -> {
            long longId = 140275;
            cb.query().setId_Equal(longId);
        });
        String namae = gakuseki.map(TblGakuseki::getShimei).orElse("名前なし");
        return String.format("名前: %s", namae);
    }
}
```

- selectEntity()メソッドの戻り値は、`org.dbflute.optional.OptionalEntity`でラップされている
- 値が入っていない場合もあるので、それに対処するコードが必要

## サンプルコード 2

```java
public class DemoApplication {

    @Autowired
    private TblGakusekiBhv tblGakusekiBhv;

    @GetMapping("/avocado")
    public String avocado() {
        AtomicReference<String> namae = new AtomicReference<String>("初期値");
        OptionalEntity<TblGakuseki> gakuseki = tblGakusekiBhv.selectEntity(cb -> {
            long longId = 1400275;
            cb.query().setId_Equal(longId);
        });
        gakuseki.ifPresent(member -> {
            namae.set(member.getShimei());
        }).orElse(() -> {
            namae.set("名前なし");
        });
        return String.format("Hello %s" , namae.get());
    }
}
```

- 原則として、ラムダ式の外で定義された変数に、ラムダ式の内で値を代入することはできない
- `java.util.concurrent.atomic.AtomicReference`を使えば、ラムダ式の内で値を挿入することができる

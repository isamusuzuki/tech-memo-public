# setupSelect()メソッドを使って外部テーブルを結合させる

作成日 2024/12/24、更新日 2025/06/05

参照サイト => [SetupSelect(Relation)](https://dbflute.seasar.org/ja/manual/function/ormapper/conditionbean/setupselect/index.html)

## 1. サンプルコード

```java
public class DemoApplication {

    @Autowired
    private TblGakusekiBhv tblGakusekiBhv;

    @GetMapping("/banana")
    public String banana() {
        OptionalEntity<TblGakuseki> gakuseki = tblGakusekiBhv.selectEntity(cb -> {
            long longId = 140275;
            cb.setupSelect_MstShozoku1();
            cb.query().queryMstShozoku1().innerJoin();
            cb.query().setId_Equal(longId);
        });
        String namae = gakuseki.map(TblGakuseki::getShimei).orElse("名前なし");
        String shozoku = gakuseki.flatMap(TblGakuseki::getMstShozoku1).map(MstShozoku1::getMeisyou).orElse("所属無し");
        return String.format("名前: %s, 所属: %s", namae, shozoku);
    }
}
```

- ConditionBean を設定している箇所でも`setupSelect`を使って外部テーブルを呼び出すし、さらに実際の値を取り出すコードでも、外部テーブルを呼び出す。これが DBFlute の流儀

## 2. 外部テーブルの外部テーブルを結合させる

- `setupSelect`に続けて、`with`を使うと、外部テーブルの外部テーブルを結合させることが可能
- `with`をチェーンさせると、さらにその先の外部テーブルという意味になってしまう
- 同じ階層にある複数の外部テーブルは以下のように、複数回`setupSelect`する必要がある

```java
ListResultBean<Entity> list = tableBhv.selectList(cb -> {
    cb.query().queryMstTableByMstId().setCode_Equal(code);

    cb.setupSelect_MstTableByMstId();
    cb.setupSelect_RelatedTable().withRelated2();
    cb.setupSelect_RelatedTable().withRelated3();
    cb.setupSelect_RelatedTable().withRelated4();
});
```

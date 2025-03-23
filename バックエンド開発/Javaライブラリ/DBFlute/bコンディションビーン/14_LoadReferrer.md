# LoadReferrer

作成日 2024/12/26、更新日 2024/12/27

参照サイト => [LoadReferrer](https://dbflute.seasar.org/ja/manual/function/ormapper/behavior/select/loadreferrer.html)

関連する子テーブルのデータを取得する

## LoadReferrer の基本概念

- まず、ConditionBean を使って、基点テーブルのデータ一覧を取得する
- 次に、LoadReferrer を使って、基点テーブルのデータに関連する全ての子テーブルのデータを一括取得する
- 取得後、LoadReferrer の処理の中で、基点テーブルのデータと子テーブルのデータをマッピングして、DomainEntity のオブジェクトグラフを構築する
- SQL の合計は、「基点テーブルの検索 1 回＋子テーブルの数」となる

## サンプルコード

```java
public class DemoApplication {

    @Autowired
    private TblGakusekiBhv tblGakusekiBhv;

  @GetMapping("/daikon")
  public String daikon() {
    StringBuilder sb = new StringBuilder();

    ListResultBean<MstShozoku1> shozoku1List = mstShozoku1Bhv.selectList(cb -> {
      List<Long> longIdList = new ArrayList<Long>(Arrays.asList(11L, 12L));
      cb.query().setId_InScope(longIdList);
    });

    mstShozoku1Bhv.loadTblGakuseki(shozoku1List, gakusekiCb -> {
      long longId = 140000;
      gakusekiCb.query().setId_GreaterThan(longId);
      gakusekiCb.query().addOrderBy_Id_Desc();
    });

    for (MstShozoku1 shozoku1 : shozoku1List) {
      sb.append(String.format("id: %s,  名称: %s, ", shozoku1.getId(), shozoku1.getMeisyou()));

      List<TblGakuseki> gakusekiList = shozoku1.getTblGakusekiList();
      sb.append(String.format("カウント: %s<br>", gakusekiList.size()));

      for (TblGakuseki gakuseki : gakusekiList) {
        sb.append(String.format("名前: %s<br>", gakuseki.getShimei()));
      }
    }
    return sb.toString();
  }
}
```

- `mstShozoku1Bhv.loadTblGakuseki()`の行をまるまるコメントアウトしても、エラーにはならない。ただし、for ループの中の`List<TblGakuseki> gakusekiList`は空になる
- LoadReferrer は、ConditionBean と同様に、設定された箇所で SQL クエリーが生成＆実行されるので、書かないとデータは取得できない
- 実際に値を取得シーンでも外部テーブルを呼び出す必要があるのは、テーブルごとにエンティティ関連のコードが自動生成されているからだと理解した

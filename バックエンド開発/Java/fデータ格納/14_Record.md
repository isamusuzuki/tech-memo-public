# Record

作成日 2025/01/28

参照サイト => [[Java16]新機能 Record の詳細](https://qiita.com/ReiTsukikazu/items/6dc3ec9ea9646c472db0)

> record は，enum と同様に特別なクラスです．

```java
// 定義
record Point(int x, int y) {}
record Sample(int i, String str){}

// 局所レコード
List<Merchant> findTopMerchants(List<Merchant> merchants, int month) {
    record MerchantSales(Merchant merchant, double sales) {}
    return merchants.stream()
        .map(merchant -> new MerchantSales(merchant, computeSales(merchant, month)))
        .sorted((m1, m2) -> Double.compare(m2.sales(), m1.sales()))
        .map(MerchantSales::merchant)
        .collect(toList());
}
```

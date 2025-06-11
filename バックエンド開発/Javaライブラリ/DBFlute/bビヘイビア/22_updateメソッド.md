# insert()メソッドを使ってデータを 1 件更新する

作成日 2025/06/11

公式サイト => [update(entity)](https://dbflute.seasar.org/ja/manual/function/ormapper/behavior/update/update.html)

> 該当のBehaviorに対応するテーブルのEntityをもとに一件更新する
>
> Setterが呼び出されたカラムに対してのみ、更新値を設定する

## サンプルコード

```java
Member member = new Member();
member.setMemberId(memberId); // PK
member.setMemberName("Stojkovic"); // 業務的な更新値の設定
memberBhv.update(member); // 戻り値はvoid
```

# insert()メソッドを使ってデータを1件登録する

作成日 2025/06/11

公式サイト => [insert(entity)](https://dbflute.seasar.org/ja/manual/function/ormapper/behavior/select/selectlist.html)

> 該当のBehaviorに対応するテーブルのEntityをもとに一件登録する
>
> 主キー(PK)の値が自動採番される設定になっている場合(シーケンスや Identity)は、主キーの値を設定せずに登録し、登録処理後のEntityから採番された主キーの値が取得できる

## サンプルコード

```java
Member member = new Member();
//member.setMemberId(1); // 自動採番の場合は不要
member.setMemberName("Stojkovic");
member.setMemberAccount("Pixy");

memberBhv.insert(member); // 戻り値はvoid

Integer memberId = member.getMemberId(); // 自動採番の場合は採番値が取得できる
```

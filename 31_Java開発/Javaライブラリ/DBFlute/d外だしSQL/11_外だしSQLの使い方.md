# [外だしSQLの使い方](https://dbflute.seasar.org/ja/manual/function/ormapper/outsidesql/howto.html)

作成日 2025/05/27

## 1. 実装の流れ

1. 外だしSQLをパラメータコメントを使って2Way-SQLで書く
1. Sql2Entityを実行して、CustomizeEntityとParameterBean(pmb)を自動生成する
1. BehaviorのoutsideSql()メソッドから実行する

## 2. SQLファイルの作成

EclipseのEMechaプラグインの利用が前提

紐づけるテーブルを決める。対応するBehaviorを開いて、右クリックでプラグインをスタート

| Key               | Value                                          |
| ----------------- | ---------------------------------------------- |
| Source folder     | `src/main/resources`までのディレクトリを書く   |
| Package           | Behaviorが所属するパッケージ名                 |
| Behavior          | Behaviorのパッケージ名＋クラス名               |
| SQL Name          | select + SQL業務名                             |
| DBFlute Options 1 | Check "Use SQL Title and Description Comment." |
| DBFlute Options 2 | Check "Use Customize Entity."                  |
| DBFlute Options 3 | Check "Use Parameter Bean."                    |
| DBFlute Options 4 | Check "Use Auto Detect."                       |

Enterボタンをクリックすると、`src/main/resources`に、パッケージ分のディレクトリが切られて、SQLファイルが生成される

SQLファイルの名前：`[紐づけテーブルBhv]_[select][SQL業務名].sql`

SQLファイルの中身（ひな形）：

```sql
/*
  [df:title]
  SQLのタイトル

  [df:description]
  SQLの説明
*/
-- #df:entity#

-- !df:pmb!
-- !!AutoDetect!

select ...
  from ...
  where ...
  order by ...;
```

## 2. 2Way-SQLの文法

- BEGINコメント（要ENDコメント）
- IFコメント（要ENDコメント）
- バインド変数コメント（必ずテスト値を入れる）

```sql
select mb.MEMBER_ID
     , mb.MEMBER_NAME
     , stat.MEMBER_STATUS_NAME
  from MEMBER mb
    left outer join MEMBER_STATUS stat
      on mb.MEMBER_STATUS_CODE = stat.MEMBER_STATUS_CODE
 /*BEGIN*/
 where
   /*IF pmb.memberId != null*/
   mb.MEMBER_ID = /*pmb.memberId*/3
   /*END*/
   /*IF pmb.memberName != null*/
   and mb.MEMBER_NAME like /*pmb.memberName*/'M%'
   /*END*/
 /*END*/
 order by mb.BIRTHDATE desc, mb.MEMBER_ID asc
```

## 3. Sql2Entityタスクの実行

DBFluteクライアント配下の manage.bat|sh から Sql2Entity を実行する

成功すると、CustomizeEntityやParameterBeanが所定のパッケージに自動生成される

- bsentity.customizeとexentity.customize配下にCustomizeEntity
- bsbhv.pmbeanとexentity.pmbean配下にParameterBean

## 4. 外だしSQLの呼び出し

BehaviorのoutsideSql()メソッドから外だしSQLを実行する

- リスト検索 `outsideSql().selectList(pmb)`
- 一件検索 `outsideSql().selectEntity(pmb)`

```java
SimpleMemberPmb pmb = new SimpleMemberPmb();
pmb.setMemberName_PrefixSearch("S");

// 外だしSQLの実行 (MemberBhv_selectSimpleMember.sql)
List<SimpleMember> memberList
        = memberBhv.outsideSql().selectList(pmb);
```

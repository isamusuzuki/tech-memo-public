# OptionalEntityクラス

作成日 2025/02/20

## 公式サイトを読み歩く

[テーブル、つまり Entity は Optional 祭り ☆](https://dbflute.seasar.org/ja/tutorial/onjava8.html#optionalparade)

> selectEntity() の戻り値、そして、setupSelect したときの関連テーブルを Entity で get するとき、もろもろとにかく Entity は OptionalEntity になっています。

[Java8 なら OptionalEntity](https://dbflute.seasar.org/ja/manual/function/ormapper/behavior/select/selectentity.html#java8)

> Java8 であれば、selectEntity()の戻り値は OptionalEntity となります。Java8 で導入された Optional を Entity に適用したものです。

[Java8 ならリレーションも Optional](https://dbflute.seasar.org/ja/manual/function/ormapper/conditionbean/setupselect/index.html#java8)

> Java8 であれば、関連テーブルの Entity を get するときの戻り値 "も" OptionalEntity となります。

## OptionalEntity と Optional の混在に要注意

### AuthenticationService.java

- コンディションビーンを使う側
- ラムダ式で得た値をラムダ式の外で使うために AtomicReference を利用
- 戻り値は Optional クラスになる

```java
public Optional<DSession> getSession(String sessionUuid) {
    AtomicReference<DSession> sessionBox = new AtomicReference<>();
    dSessionBhv.selectEntity(cb -> {
        cb.query().setSessionUuid_Equal(sessionUuid);
        cb.setupSelect_DUser();
    }).ifPresent(session -> {
        sessionBox.set(session);
    });
    return Optional.ofNullable(sessionBox.get());
}
```

### UserDetailsServiceImpl.java

- 戻り値を利用する側
- 関連テーブルの Entity を get したときの戻り値は OptionalEntity になる
- toOptional()メソッドで Optional に変換してもいいが、それ以降のコードに変化なし

```java
Optional<DSession> dSession = authenticationService.getSession(username);
if (dSession.isPresent()) {
    OptionalEntity<DUser> dUser = dSession.get().getDUser();
    // Optional<DUser> dUser = dSession.get().getDUser().toOptional();
    if (dUser.isPresent()) {
        String userCode = dUser.get().getUserCode();
        String userPass = "*";
        String userRole = "ROLE_" + dUser.get().getUserType().toUpperCase();
    }
}
```

# ResponseEntityクラス

作成日 2025/02/12

## 1. javadocを読む

[クラス ResponseEntity\<T\>](https://spring.pleiades.io/spring-framework/docs/current/javadoc-api/org/springframework/http/ResponseEntity.html)

- `org.springframework.http.ResponseEntity<T>`
- HttpStatusCodeステータスコードを追加するHttpEntityの拡張。RestTemplateおよび@Controllerメソッドで使用されます。

## 2. 参照サイトを読む

[ResponseEntity クラスについて](https://qiita.com/kaede25/items/704832692e0db702c435)

ResponseEntity の要素

1. HTTP ステータスコード
1. レスポンスボディ（任意のオブジェクト）=> JSON や XML に変換される
1. HTTP ヘッダー

ビルダーパターンを使ったレスポンス構築

```java
@GetMapping("/builder")
public ResponseEntity<String> getResponseUsingBuilder() {
    return ResponseEntity
            .status(HttpStatus.OK)
            .header("Custom-Header", "HeaderValue")
            .body("Response using ResponseEntity builder");
}
```

## 3. 参照サイト2を読む

[[Java&SpringBoot] いろいろアノテーションと ResponseEntity](https://note.com/commonerd/n/nbf44acd9e14f)

```java
@RestController
public class StudentController {
    @GetMapping("students")
    public ResponseEntity<List<Student>> getStudents(){
        List<Student> students = new ArrayList<>();
        students.add(new Student(1, "Ramesh", "Fadatare"));
        students.add(new Student(2, "umesh", "Fadatare"));
        students.add(new Student(3, "Ram", "Jadhav"));
        students.add(new Student(4, "Sanjay", "Pawar"));
        return ResponseEntity.ok(students);
    }
}
```

## 4. 自分で確認したこと

確かに何も指定しなくても`Content-Type: application/json`でデータが送られてきた

```java
// 単純パターン
@GetMapping("/myfriends")
public ResponseEntity<List<MyFriendsEntity>> getAll() {
    List<MyFriendsEntity> list = myFriendsService.findAllMyFriends();
    return ResponseEntity.ok(list);
}
// ビルダーパターン
@GetMapping("/myfriends")
public ResponseEntity<List<MyFriendsEntity>> getAll() {
    List<MyFriendsEntity> list = myFriendsService.findAllMyFriends();
    return ResponseEntity
        .status(HttpStatus.OK)
        .header("Custom-Header", "HiHo")
        .body(list);
}
```

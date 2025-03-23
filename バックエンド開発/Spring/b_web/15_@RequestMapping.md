# RequestMapping

作成日 2025/02/13、更新日 2025/03/04

- `@RequestMapping` ... 主にクラスとクラスパスを紐づける役割を担う
- `@GetMapping` ... メソッドと GET の処理を行う URL を紐づける役割を担う

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/test")
// このクラスへのリクエストのURLは http://ドメイン/api/test/xxx となる
public class TestController {

    @GetMapping("/public/hello")
    // このメソッドへのリクエストのURLは http://ドメイン/api/test/public/hello となる
    public String hello()
    {
        return "Hello";
    }
}
```

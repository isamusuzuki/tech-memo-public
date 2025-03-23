# Spring Quickstart Guide を写経する

作成日 2024/12/24

## [英語ホームページ](https://spring.io/quickstart)を写経する

- ブラウザで[Spring Initializer](https://start.spring.io/)を開く
- 次の設定に変更する

| Key              | Value                   |
| ---------------- | ----------------------- |
| Project          | Maven                   |
| Language         | Java                    |
| Spring Boot      | 3.4.1                   |
| Project Metadata | (初期値のまま)          |
| Packaging        | Jar                     |
| Java             | 21                      |
| Dependencies     | "Spring Web" を追加する |

- GENERATE ボタンをクリックすると、`demo.zip`がダウンロードされる
- 解凍して、Visual Studio Code でフォルダを開く
- `src/main/java/com/example/demo/DemoApplication.java`ファイルを編集する

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
// importを3行追加。最後のクラス名を書けば候補が登場する
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
// RestControllerアノテーションを追加
@RestController
public class DemoApplication {

 public static void main(String[] args) {
  SpringApplication.run(DemoApplication.class, args);
 }

 // GetMappingアノテーションを追加するとメソッドが自動追加される
 @GetMapping("/hello")
 public String hello(@RequestParam(value = "name", defaultValue = "World") String name) {
  return String.format("Hello %s!" , name);
 }
}
```

- ターミナルを開いて、`./mnvw spring-boot:run`
- ブラウザを開いて、[http://localhost:8080/hello](http://localhost:8080/hello)
- リクエストパラメーターを追加する、[http://localhost:8080/hello?name=John](http://localhost:8080/hello?name=John)

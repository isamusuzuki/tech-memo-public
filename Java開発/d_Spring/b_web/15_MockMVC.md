# MockMVCを使ったコントローラーの単体テスト

作成日 2025/03/04、更新日 2025/03/06

参照書籍 => P.290 「第 28 章 Controller のユニットテスト」 from 『プロになるための Spring 入門』

## 1. MockMVCとは

- Controller に疑似的なリクエストを送信することができる
- Spring のテストサポート機能が提供する仕組み
- Spring Initializr で Spring Web を追加していればインストールされている
- AP サーバーを起動する必要がないので、テストの実行が速くなる

## 2. 公式ガイド（日本語）を読む

[MockMvc と @MockBean で Web レイヤーテスト](https://spring.pleiades.io/guides/gs/testing-web)

### 2a. 依存関係がないコントローラーをテストする

```text
--src/
    |--main/java/com/example/testingweb/
    |   `--HomeController.java
    `--test/java/com/example/testingweb/
        `--TestingWebApplicationTest.java
```

#### HomeController.java

```java
package com.example.testingweb;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HomeController {

    @RequestMapping("/")
    public @ResponseBody String greeting() {
        return "Hello, World";
    }

}
```

#### TestingWebApplicationTest.java

```java
package com.example.testingweb;

import static org.hamcrest.Matchers.containsString;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.jupiter.api.Test;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.servlet.MockMvc;

@SpringBootTest
@AutoConfigureMockMvc
class TestingWebApplicationTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void shouldReturnDefaultMessage() throws Exception {
        this.mockMvc.perform(get("/")).andDo(print()).andExpect(status().isOk())
                .andExpect(content().string(containsString("Hello, World")));
    }
}
```

- このテストでは、完全な Spring アプリケーションコンテキストが開始されるが、サーバーはない

### 2b. 依存関係があるコントローラーをテストする

```text
--src/
    |--main/java/com/example/testingweb/
    |   |--GreetingController.java
    |   `--GreetingService.java
    `--test/java/com/example/testingweb/
        `--WebMockTest.java
```

#### GreetingController.java

```java
package com.example.testingweb;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;


@Controller
public class GreetingController {

    private final GreetingService service;

    public GreetingController(GreetingService service) {
        this.service = service;
    }

    @RequestMapping("/greeting")
    public @ResponseBody String greeting() {
        return service.greet();
    }

}
```

#### GreetingService.java

```java
package com.example.testingweb;

import org.springframework.stereotype.Service;

@Service
public class GreetingService {
    public String greet() {
        return "Hello, World";
    }
}
```

#### WebMockTest.java

```java
package com.example.testingweb;

import static org.hamcrest.Matchers.containsString;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.jupiter.api.Test;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.test.web.servlet.MockMvc;

@WebMvcTest(GreetingController.class)
class WebMockTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private GreetingService service;

    @Test
    void greetingShouldReturnMessageFromService() throws Exception {
        when(service.greet()).thenReturn("Hello, Mock");
        this.mockMvc.perform(get("/greeting")).andDo(print()).andExpect(status().isOk())
                .andExpect(content().string(containsString("Hello, Mock")));
    }
}
```

- テストクラスに`@WebMvcTest`を付ける。このアノテーションはDIコンテナを自動で生成する
- そのDIコンテナは、`@Service`, `@Repository`のアノテーションがついたクラスのBean定義を無効にする
- 括弧の中で、テスト対象のControllerクラスを指定する
- Mockitoが提供する`@Mock`ではなく、Springが提供する`@MockBean`を使う

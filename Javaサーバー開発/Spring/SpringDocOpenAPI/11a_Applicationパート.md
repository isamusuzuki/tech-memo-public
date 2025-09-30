# Aplication パート（サンプルコードから）

作成日 2025/03/07、更新日 2025/04/03

- `@OpenAPIDefinition` ... Swagger-UI のページタイトル
- `@Info` ... title, version, description

## src/main/kotlin/com/example/demo/DemoApplication.kt

※ Javaコードに変換済み

```java
package com.example.demo;

import io.swagger.v3.oas.annotations.OpenAPIDefinition;
import io.swagger.v3.oas.annotations.info.Info;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.runApplication;

@OpenAPIDefinition(
    info = @Info(
        title = "Springdoc-openapi sample",
        version = "1.0",
        description = "Springdoc-openapiのサンプルです"
    )
)
@SpringBootApplication
class SpringdocOpenapiSampleApplication {
}
```

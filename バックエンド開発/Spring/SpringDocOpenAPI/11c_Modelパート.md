# Model パート（サンプルコードから）

作成日 2025/03/07、更新日 2025/04/03

- `@Schema` ... description, example

## src/main/kotlin/com/example/demo/model/ErrorResponse.kt

※ Javaコードに変換済み

```java
package com.example.demo.model;

import io.swagger.v3.oas.annotations.media.Schema;

public class ErrorResponse {
    @Schema(description = "エラーコード", example = "NOT_FOUND")
    public String code;

    @Schema(description = "エラーメッセージ", example = "指定されたリソースが見つかりません")
    public String message;

    @Schema(description = "エラーの詳細情報", example = "ユーザーID: 12345 は存在しません")
    public String details;
}
```

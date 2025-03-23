# Model パート（サンプルコードから）

作成日 2025/03/07

- `@Schema` ... description, example

## src/main/kotlin/com/example/demo/model/ErrorResponse.kt

```kotlin
package com.example.demo.model

import io.swagger.v3.oas.annotations.media.Schema

data class ErrorResponse(
    @Schema(description = "エラーコード", example = "NOT_FOUND")
    val code: String,

    @Schema(description = "エラーメッセージ", example = "指定されたリソースが見つかりません")
    val message: String,

    @Schema(description = "エラーの詳細情報", example = "ユーザーID: 12345 は存在しません")
    val details: String?
)
```

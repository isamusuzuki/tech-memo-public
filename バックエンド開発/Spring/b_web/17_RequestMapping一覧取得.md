# RequestMappingの一覧を取得する

作成日 2025/08/26、更新日 2025/08/27

## 1. 概要

- `RequestMappingHandlerMapping`クラス ... `@RequestMapping`アノテーションから`RequestMappinpInfo`インスタンスを作成する
- `RequestMappingInfo`クラス ... リクエストマッピング情報（HTTPメソッドとパスの分解は、データ分析で実行する）
- `HandlerMethod`クラス ... メソッドとBeanで構成されるハンドラーメソッドに関する情報をカプセル化する

## 2. サンプルコード

※ Spring Framework 2.7のシステムで書いたので、インポートに`javax`が登場する

```java
package com.example.controllers;

import java.io.IOException;
import java.io.OutputStream;
import java.nio.charset.StandardCharsets;
import java.util.Map;
import java.util.StringJoiner;

import javax.servlet.http.HttpServletResponse;

import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.mvc.method.RequestMappingInfo;
import org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping;

@Controller
public class ZuluController {
    private final RequestMappingHandlerMapping requestMappingHandlerMapping;

    public ZuluController(@Qualifier("requestMappingHandlerMapping") RequestMappingHandlerMapping requestMappingHandlerMapping) {
        this.requestMappingHandlerMapping = requestMappingHandlerMapping;
    }

    @GetMapping("/zulu")
    public void zulu(HttpServletResponse response) throws IOException {
        response.addHeader("Content-Disposition", "attachment; filename=\"requestMapping.csv\"");
        Map<RequestMappingInfo, HandlerMethod> handlerMethodMap = this.requestMappingHandlerMapping.getHandlerMethods();
                try (OutputStream os = response.getOutputStream()) {
            StringJoiner sj = new StringJoiner("\n");
            sj.add("RequestMapping,ClassName,MethodName");
            for ( Map.Entry<RequestMappingInfo, HandlerMethod> entry : handlerMethodMap.entrySet() ) {
                RequestMappingInfo info = entry.getKey();
                HandlerMethod method = entry.getValue();
                sj.add(info + "," + method.getMethod().getDeclaringClass().getName() + "," + method.getMethod().getName());
            }
            os.write(sj.toString().getBytes(StandardCharsets.UTF_8));
            os.flush();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

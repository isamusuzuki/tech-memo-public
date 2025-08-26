# マッピングされているURLの一覧を取得したい

作成日 2025/08/26

## 1. 情報を探す

[Spring、TERASOLUNA でマッピングされているURLの一覧を確認したい。](https://qiita.com/otsu-Miya/items/eac37b2720b67a58690b)

[Spring Boot でマッピングされているURLの一覧を確認したい](https://qiita.com/takemitsu/items/94e1adf98404ee2ec17d)

サンプルコードをコピーして、デバッグしてみる

```java
public class RootController {

    private RequestMappingHandlerMapping handlerMapping;

    public RootController(RequestMappingHandlerMapping handlerMapping) {
        this.handlerMapping = handlerMapping;
    }

    @GetMapping("/test")
    public ResponseEntity<String> test() throws ClassNotFoundException {
        log.info("===== url mapping");
        Map<RequestMappingInfo, HandlerMethod> handlerMethodMap = this.handlerMapping.getHandlerMethods();
        for ( Map.Entry<RequestMappingInfo, HandlerMethod> entry : handlerMethodMap.entrySet() ) {
            RequestMappingInfo info = entry.getKey();
            HandlerMethod method = entry.getValue();
            log.info("  " + info + " => " + method.getMethod().getDeclaringClass().getName() + "#" + method.getMethod().getName());
        }
        return ResponseEntity.ok("<h1>fileout test page</h1>");
    }
}
```

エラーが発生する

```text
Parameter 0 of constructor in jp.co.ailesys.fileout.controllers.RootController required a single bean, but 2 were found:
        - requestMappingHandlerMapping: defined by method 'requestMappingHandlerMapping' in class path resource [org/springframework/boot/autoconfigure/web/servlet/WebMvcAutoConfiguration$EnableWebMvcConfiguration.class]
        - controllerEndpointHandlerMapping: defined by method 'controllerEndpointHandlerMapping' in class path resource [org/springframework/boot/actuate/autoconfigure/endpoint/web/servlet/WebMvcEndpointManagementContextConfiguration.class]

This may be due to missing parameter name information
```

## 2. 対処法を探す

[requestMappingHandlerMapping has 2 potential beans when actuator is active from spring boot version 2.7](https://github.com/spring-projects/spring-boot/issues/31961)

動いたコード

```java
public class RootController {

    RequestMappingHandlerMapping handlerMapping;

    public RootController(@Qualifier("requestMappingHandlerMapping") RequestMappingHandlerMapping handlerMapping) {
        this.handlerMapping = handlerMapping;
    }

    @GetMapping("/test")
    public ResponseEntity<String> test() throws ClassNotFoundException {
        log.info("===== url mapping");
        Map<RequestMappingInfo, HandlerMethod> handlerMethodMap = this.handlerMapping.getHandlerMethods();
        for ( Map.Entry<RequestMappingInfo, HandlerMethod> entry : handlerMethodMap.entrySet() ) {
            RequestMappingInfo info = entry.getKey();
            HandlerMethod method = entry.getValue();
            log.info("  " + info + " => " + method.getMethod().getDeclaringClass().getName() + "#" + method.getMethod().getName());
        }
        return ResponseEntity.ok("<h1>fileout test page</h1>");
    }
}
```

## 3. テキストファイルとして出力させる

```java
public class RootController {

    RequestMappingHandlerMapping handlerMapping;

    public RootController(@Qualifier("requestMappingHandlerMapping") RequestMappingHandlerMapping handlerMapping) {
        this.handlerMapping = handlerMapping;
    }

    @GetMapping("/test")
    public void test(HttpServletResponse response) throws IOException {
        response.addHeader("Content-Disposition", "attachment; filename=\"mapping.csv\"");

        Map<RequestMappingInfo, HandlerMethod> handlerMethodMap = this.handlerMapping.getHandlerMethods();
        
        try (OutputStream os = response.getOutputStream()) {
            StringJoiner sj = new StringJoiner("\n");
            sj.add("RequestMapping,HandlerMethod");
            for ( Map.Entry<RequestMappingInfo, HandlerMethod> entry : handlerMethodMap.entrySet() ) {
                RequestMappingInfo info = entry.getKey();
                HandlerMethod method = entry.getValue();
                sj.add(info + "," + method.getMethod().getDeclaringClass().getName() + "#" + method.getMethod().getName());
            }
            os.write(sj.toString().getBytes(StandardCharsets.UTF_8));
            os.flush();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

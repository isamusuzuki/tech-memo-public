# QueryMappingの一覧を取得する

作成日 2025/08/27

## 1. 概要

- ActuatorのMappingsエンドポイントは、`@RequestMapping`のパスのリストしか表示しない
- Actuatorのエンドポイントに、GraphQLの項目はない
- Springアプリケーションに登録されている全てのBeanを管理する`ApplicationContext`から情報を取得する

## 2. サンプルコード

※ Spring Framework 2.7のシステムで書いたので、インポートに`javax`が登場する

```java
package com.example.controllers;

import java.io.IOException;
import java.io.OutputStream;
import java.lang.reflect.Method;
import java.nio.charset.StandardCharsets;
import java.util.Map;
import java.util.StringJoiner;

import javax.servlet.http.HttpServletResponse;

import org.springframework.aop.support.AopUtils;
import org.springframework.context.ApplicationContext;
import org.springframework.core.MethodIntrospector;
import org.springframework.core.annotation.AnnotatedElementUtils;
import org.springframework.graphql.data.method.annotation.QueryMapping;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class ZuluController {
    private final ApplicationContext context;

    public ZuluController(ApplicationContext context) {
        this.context = context;
    }

    @GetMapping("/zulu")
    public void zulu(HttpServletResponse response) throws IOException {
        response.addHeader("Content-Disposition", "attachment; filename=\"queryMapping.csv\"");
        Map<String, Object> controllers = context.getBeansWithAnnotation(Controller.class);
        
        try (OutputStream os = response.getOutputStream()) {
            StringJoiner sj = new StringJoiner("\n");
            sj.add("QueryName,ClassName,MethodName");
            controllers.forEach((beanName, beanInstance) -> {
                // AOPプロキシを考慮して、元のクラスを取得
                final Class<?>  targetClass = AopUtils.getTargetClass(beanInstance);

                // クラス内のメソッドを探索し、@QueryMappingアノテーションを持つメソッドを取得
                Map<Method, QueryMapping> annotatedMethods = MethodIntrospector.selectMethods(
                    targetClass, 
                    (MethodIntrospector.MetadataLookup<QueryMapping>) method -> 
                    AnnotatedElementUtils.findMergedAnnotation(method, QueryMapping.class)
                );
                
                if (!annotatedMethods.isEmpty()) {
                    annotatedMethods.forEach((method, queryMapping) -> {
                        // @QueryMappingのname属性（GraphQLフィールド名）を取得
                        // name属性が未指定の場合はメソッド名が使われる
                        String queryName = queryMapping.name();
                        if (queryName.isEmpty()) {
                            queryName = method.getName();
                        }
                        sj.add(queryName + "," + targetClass.getName() + "," + method.getName());
                    });                    
                }   
            });
            os.write(sj.toString().getBytes(StandardCharsets.UTF_8));
            os.flush();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

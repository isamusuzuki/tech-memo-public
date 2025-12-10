# Gradleを試す

作成日 2025/12/09

## 1. [Spring Initializr](https://start.spring.io/)を使う

| Key              | Value                  |
| ---------------- | ---------------------- |
| Project          | Gradle - Groovy        |
| Language         | Java                   |
| Spring Boot      | 4.0.0                  |
| Project Metadata | (初期値のまま)         |
| Packaging        | Jar                    |
| Configuration    | Properties             |
| Java             | 21                     |
| Dependencies     | "Spring Web"を追加する |

- GENERATEボタンをクリックすると、`demo.zip`がダウンロードされる
- 解凍してdemoフォルダを取り出し、Visual Studio Codeで開く

## 2. 開発コンテナを用意する

.devcontainer/devcontainer.json

```json
{
    "name": "Java",
    "image": "mcr.microsoft.com/devcontainers/java:1-21-bookworm",
    "features": {
        "ghcr.io/devcontainers/features/java:1": {
            "version": "none",
            "installMaven": "false",
            "installGradle": "true"
        }
    },
    "customizations": {
        "vscode": {
            "extensions": ["vscjava.vscode-java-pack", "vmware.vscode-boot-dev-pack"],
            "settings": {
                "java.compile.nullAnalysis.mode": "automatic",
                "java.configuration.updateBuildConfiguration": "interactive"
            }
        }
    }
}
```

## 2. Javaコードを編集する

src/main/java/com/example/demo/DemoApplication.java

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.view.RedirectView;

@SpringBootApplication
@RestController
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @GetMapping("/")
    public RedirectView root() {
        return new RedirectView("/hello");
    }

    @GetMapping("/hello")
    public String hello(@RequestParam(defaultValue = "World") String name) {
        return String.format("Hello, %s!", name);
    }
}
```

## 4. デバッグする

.vscode/launch.json

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "java",
            "name": "DemoApplication",
            "request": "launch",
            "mainClass": "com.example.demo.DemoApplication",
            "projectName": "demo",
            "args": "--port=8080",
            "vmArgs": "-Dspring.profiles.active=dev",
            "serverReadyAction": {
                "pattern": "Started DemoApplication in [0-9.]+ seconds",
                "action": "startDebugging",
                "name": "Launch Chrome",
                "killOnServerStop": true
            }
        },
        {
            "name": "Launch Chrome",
            "request": "launch",
            "type": "chrome",
            "url": "http://localhost:8080",
            "webRoot": "${workspaceFolder}"
        }
    ]
}
```

## 5. gradleコマンドを試す

```bash
# バージョンを表示する
./gradlew --version

# 全タスクを表示する
./gradlew tasks

# テストタスクを実行する
./gradlew test
```

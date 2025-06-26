# Mockito

作成日 2025/02/26、更新日 2025/02/28

## 1. Mockito を使うと デバッグコンソールにメッセージが出る

```text
Mockito is currently self-attaching to enable the inline-mock-maker. This will no longer work in future releases of the JDK. Please add Mockito as an agent to your build what is described in Mockito's documentation: https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#0.3
WARNING: A Java agent has been loaded dynamically (C:\Users\ユーザー名\.m2\repository\net\bytebuddy\byte-buddy-agent\1.15.11\byte-buddy-agent-1.15.11.jar)
WARNING: If a serviceability tool is in use, please run with -XX:+EnableDynamicAgentLoading to hide this warning
WARNING: If a serviceability tool is not in use, please run with -Djdk.instrument.traceUsage for more information
WARNING: Dynamic loading of agents will be disallowed by default in a future release
OpenJDK 64-Bit Server VM warning: Sharing is only supported for boot loader classes because bootstrap classpath has been appended
```

[0.3. Explicitly setting up instrumentation for inline mocking (Java 21+)](https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#0.3)

> Starting from Java 21, the JDK restricts the ability of libraries to attach a Java agent to their own JVM. As a result, the inline-mock-maker might not be able to function without an explicit setup to enable instrumentation, and the JVM will always display a warning.

## 2. 参考記事を読む

[Mockito is currently self-attaching to enable the inline-mock-maker. This will no longer work in future releases of the JDK](https://stackoverflow.com/questions/79278490/mockito-is-currently-self-attaching-to-enable-the-inline-mock-maker-this-will-n)

argLine に以下の 2 行を追加せよ

- `-javaagent:${settings.localRepository}/org/mockito/mockito-core/${mockito.version}/mockito-core-${mockito.version}.jar`
- `-Xshare:off`

```text
~/.m2/respository/org/mockito/mockito-core/
    |--4.5.1/mockito-core-4.5.1.jar    ... 2024/12/20
    |--5.11.0/mockito-core-5.11.0.jar  ... 2025/02/07
    `--5.14.2/mockito-core-5.14.2.jar  ... 2024/12/23
```

### 2a. 作業結果

pom.xml に設定を追加してもなにも変化がない。おそらく VSCode のテスト実行は Maven コマンドを使っていない

## 3. [Test Runner for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-test)の Overview を読む

Customize Test Configuration

```json
{
    "java.test.config" {
        "name": "myTestConfiguration",
        "vmArgs": [ "-Xmx512M" ],
        "env": { "key": "value" }
    }
}
```

### 3a. 作業結果

メッセージをよく読めば、明示する必要がある jar ファイルは、mockit-core ではなく、byte-buddy-agent だった

```json
"java.test.config": {
    "name": "myTestConfiguration",
    "vmArgs": [
        "-javaagent:C:\\Users\\ユーザー名\\.m2\\repository\\net\\bytebuddy\\byte-buddy-agent\\1.15.11\\byte-buddy-agent-1.15.11.jar",
        "-Xshare:off"
    ]
}
```

### 3b. byte-buddy-agent とは

[Byte Buddy Agent](https://mvnrepository.com/artifact/net.bytebuddy/byte-buddy-agent)

> The Byte Buddy agent offers convenience for attaching an agent to the local or a remote VM.

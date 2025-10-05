# SuperPOM

作成日 2025/02/18

[Super POM](https://maven.apache.org/ref/3.9.9/maven-model-builder/super-pom.html)

- Super POM は、全ての POM の最上位に位置する
- 親 POM として継承されるデフォルトの設定

## SuperPOM 抜粋

```xml
<project>
  <repositories>
    <repository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
    </repository>
  </repositories>

  <build>
    <directory>${project.basedir}/target</directory>
    <outputDirectory>${project.build.directory}/classes</outputDirectory>
    <finalName>${project.artifactId}-${project.version}</finalName>
    <sourceDirectory>${project.basedir}/src/main/java</sourceDirectory>
</project>
```

# 開発コンテナ例 Java

作成日 2025/02/11、更新日 2025/12/10

## 1. 概要

- Mavenがインストールされた状態のJava開発環境が用意される
- コンテナで再度プロジェクトを開いた後は、Java開発に必要な機能拡張がインストールされている
- コマンドパレットで、"Java: Create Java Project..."コマンドを実行できる
- そのコマンドはpom.xmlを生成し、ディレクトリ構造を整えて、最初のJavaファイルを生成する

## 2. ファイル＆フォルダ構造

```text
--avocado/
    |--.devcontainer/
    |   `--devcontainer.json
    `--demo
        |--src/main/java/com/example/
        |   `--Main.java
        `--pom.xml
```

devcontainer.json

```json
{
    "name": "Java",
    "image": "mcr.microsoft.com/devcontainers/java:1-21",
    "features": {
        "ghcr.io/devcontainers/features/java:1": {
            "version": "none",
            "installMaven": "true",
            "mavenVersion": "3.8.6",
            "installGradle": "false"
        }
    },
    "customizations": {
        "vscode": {
            "extensions": [
               "vscjava.vscode-java-pack"
            ],
            "settings": {}
        }
    }
}
```

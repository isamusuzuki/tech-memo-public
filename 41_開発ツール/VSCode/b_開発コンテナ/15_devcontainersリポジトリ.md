# devcontainersリポジトリ

作成日 2025/12/25

## evcontainersリポジトリとは？

マイクロソフトが運営している、VSCode開発コンテナ向けのコンテンツ＆設定群

ホームページ => [Development Containers](https://containers.dev/)

リポジトリ => [devcontainers](https://github.com/devcontainers)

## images ベースとなるイメージ

[https://github.com/devcontainers/images/tree/main/src/base-debian](https://github.com/devcontainers/images/tree/main/src/base-debian)

```json
{
    "image": "mcr.microsoft.com/devcontainers/base:bookworm"
}
```

## features 追加パッケージ

[https://github.com/devcontainers/features/tree/main/src/docker-in-docker](https://github.com/devcontainers/features/tree/main/src/docker-in-docker)

```json
"features": {
    "ghcr.io/devcontainers/features/docker-in-docker:2": {}
}
```

[https://github.com/devcontainers/features/tree/main/src/java](https://github.com/devcontainers/features/tree/main/src/java)

```json
"features": {
    "ghcr.io/devcontainers/features/java:1": {}
}
```

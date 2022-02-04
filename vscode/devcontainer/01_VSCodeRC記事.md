# VS Code Remote Containers の記事を読む

作成日 2022/01/03、更新日 2022/02/04

## Docker と VS Code Remote Containers を用いたフロントエンド開発環境構築

記事 => [https://zenn.dev/leftletter/articles/0969dcef061ff8](https://zenn.dev/leftletter/articles/0969dcef061ff8)

リポジトリ => [https://github.com/LeftLetter/vscode-compose-template](https://github.com/LeftLetter/vscode-compose-template)

```text
vcode-compose-template/
    |--server/
    |   `--.devcontainer/
    |       |--Dockerfile
    |       |--devcontainer.json
    |       `--swagger.yml
    |--ui/
    |   |--.devcontainer/
    |   |   |--Dockerfile
    |   |   `--devcontainer.json
    |   `--.vscode/
    |       `--launch.json
    `--docker-compose.yml
```

## devconteriner.json とは

Visual Studio Code が採用した設定ファイル

devcontainer.json reference\
[https://code.visualstudio.com/docs/remote/devcontainerjson-reference](https://code.visualstudio.com/docs/remote/devcontainerjson-reference)

> A devcontainer.json file in your project tells Visual Studio Code (and other services and tools that support the format) how to access (or create) a development container with a well-defined tool and runtime stack. It's currently supported by the Remote - Containers extension and GitHub Codespaces.

Create a development container\
[https://code.visualstudio.com/docs/remote/create-dev-container](https://code.visualstudio.com/docs/remote/create-dev-container)

> VS Code's container configuration is stored in a devcontainer.json file.
>
> The dev container configuration is either located under .devcontainer/devcontainer.json or stored as a .devcontainer.json file (note the dot-prefix) in the root of your project.

```json
{
  "image": "mcr.microsoft.com/vscode/devcontainers/typescript-node:0-12",
  "extensions": ["dbaeumer.vscode-eslint"],
  "forwardPorts": [3000]
}
```

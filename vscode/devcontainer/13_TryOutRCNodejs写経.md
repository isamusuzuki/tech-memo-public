# Try Out Development Containers: Node.js を写経する

作成日 2022/01/27

[https://github.com/microsoft/vscode-remote-try-node](https://github.com/microsoft/vscode-remote-try-node)

> A development container is a running Docker container with a well-defined tool/runtime stack and its prerequisites. You can try out development containers with GitHub Codespaces or Visual Studio Code Remote - Containers.

## Setting up the development container

- このリポジトリを自分のローカルにクローンしてくる
- Docker チュートリアルが教えてくれたコンテナ内の Git コマンドを使ってみる
- PowerShell を開く

```bash
docker run --name clone1 alpine/git clone https://github.com/microsoft/vscode-remote-try-node

docker cp clone1:/git/vscode-remote-try-node/ .
```

- VSCode で、新しいウィンドウを開く
- 左下の緑部分をクリックして、コマンドパレットを表示する
- `Try a Development Container Sample...`を選ぶ
- Node を選ぶ

左下の緑部分には、`Dev Container: Node.js` と表示された

ターミナルには、`node -> /workspaces/vscode-remote-try-node (main) $` と表示された

## Things to try

- sever.js の 20 行目にブレイクポイントを設置する
- F5 キーを押して、デバッグを開始する
- ブレイクポイントでいったん停止することを確認する
- 続行してはじめて、ブラウザに表示されることを確認する

## ファイル・フォルダ構造

```text
--vscode-remote-try-node
    |--.devcontainer/
    |   |--devcontainer.json
    |   `--Dockerfile
    |--.vscode
    |   `--launch.json
    |--.eslintrc.json
    |--.gitattributes
    |--.gitignore
    |--package.json
    |--server.js
    `--yarn.lock
```

devcontainer.json

```json
{
  "name": "Node.js",
  "build": {
    "dockerfile": "Dockerfile",
    "args": { "VARIANT": "16-bullseye" }
  },
  "settings": {},
  "extensions": ["dbaeumer.vscode-eslint"],
  "portsAttributes": {
    "3000": {
      "label": "Hello Remote World",
      "onAutoForward": "notify"
    }
  },
  "postCreateCommand": "yarn install",
  "remoteUser": "node"
}
```

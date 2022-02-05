# docker-compose を使う

作成日 2022/02/05

## 参考記事

[https://dev.classmethod.jp/articles/add-vs-code-remote-development-settings-to-existing-docker-environment/](https://dev.classmethod.jp/articles/add-vs-code-remote-development-settings-to-existing-docker-environment/)

devcontainer.json

```json
{
  "name": "dev-container", // コンテナ表示名
  "dockerComposeFile": [
    "../docker-compose.yml" // Docker Composeのファイルパス
  ],
  "service": "dev", // Docker Composeの接続サービス名
  "remoteUser": "dev-user", // デフォルトユーザをrootから切り替える
  "workspaceFolder": "/home/dev-user", // Workspaceのフォルダを指定
  "extensions": [
    // コンテナ内でインストールするVS Codeの拡張機能ID
    "ms-python.python",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "kddejong.vscode-cfn-lint"
  ],
  "settings": {
    // コンテナ内に追加するVS Codeの設定
    "python.linting.pylintEnabled": false,
    "python.linting.flake8Enabled": true,
    "editor.formatOnSave": true,
    "python.linting.flake8Args": ["--max-line-length=150"],
    "eslint.workingDirectories": [{ "mode": "auto" }],
    "cfnLint.path": "/home/jmc-dev/.pyenv/shims/cfn-lint"
  }
}
```

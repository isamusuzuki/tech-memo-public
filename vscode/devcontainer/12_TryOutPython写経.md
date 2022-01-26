# Try Out Development Containers: Python を写経する

作成日 2022/01/26

[https://github.com/microsoft/vscode-remote-try-python](https://github.com/microsoft/vscode-remote-try-python)

> A development container is a running Docker container with a well-defined tool/runtime stack and its prerequisites. You can try out development containers with GitHub Codespaces or Visual Studio Code Remote - Containers.

## Setting up the development container

VS Code の Remote Containers のドキュメント => [https://code.visualstudio.com/docs/remote/containers](https://code.visualstudio.com/docs/remote/containers)

Containers Tutorial => [https://code.visualstudio.com/docs/remote/containers-tutorial](https://code.visualstudio.com/docs/remote/containers-tutorial)

PowerShell を開く

```bash
# Check Docker
docker --version
# => Docker version 20.10.12, build e91ed57
```

- `Remote - Containers` 拡張機能をインストールする
- VSCode の新しいウィンドウを開く
- 左下の緑部分をクリックすると、コマンドバーが登場する

> - Open Folder in Container...
> - Clone Repository in Container Volume...
> - Attach to Running Container...
> - Add Development Container Configuration Files...
> - Try a Development Container Sample...
> - Getting Started with Remote-Containers

Try を選択すると、言語別のサンプルコードが選べる。Python を選ぶ。コンテナがビルドされるまで待つ

インストールされた機能拡張を使えるようにするため、VSCode をリロードする

左下の緑部分には、`Dev Container: Python 3` と表示された

ターミナルには、`vscode -> /workspaces/vscode-remote-try-python (main) $` と表示された

```bash
pwd
# => /workspaces/vscode-remote-try-python

cat /etc/os-release
# => PRETTY_NAME="Debian GNU/Linux 11 (bullseye)"
# => NAME="Debian GNU/Linux"
# => VERSION_ID="11"

python --version
# => Python 3.9.10
```

左枠 ＞ エクスプローラー

```text
--vscode-remote-try-python
    |--.devcontainer/
    |   |--devcontainers.json
    |   `--Dockerfile
    |--.vscode
    |   `--launch.json
    |--static/
    |   `--index.html
    |--.gitattributes
    |--.gitignore
    |--app.py
    `--requirements.txt
```

F5 キー（デバッグの開始）を押す ＞ ブラウザで `localhost:9000` を開いたら、ページが表示された

`Ctrl+C`でデバッグを終了する

## .devcontainers/devcontainers.json を見てみる

```json
{
  "name": "Python 3",
  "build": {
    "dockerfile": "Dockerfile",
    "context": "..",
    "args": {
      "VARIANT": "3.9-bullseye",
      "NODE_VERSION": "lts/*"
    }
  },
  "settings": {
    "terminal.integrated.profiles.linux": {
      "bash": {
        "path": "/bin/bash"
      }
    },
    "python.defaultInterpreterPath": "/usr/local/bin/python",
    "python.languageServer": "Default",
    "python.linting.enabled": true,
    "python.linting.pylintEnabled": true,
    "python.formatting.autopep8Path": "/usr/local/py-utils/bin/autopep8",
    "python.formatting.blackPath": "/usr/local/py-utils/bin/black",
    "python.formatting.yapfPath": "/usr/local/py-utils/bin/yapf",
    "python.linting.banditPath": "/usr/local/py-utils/bin/bandit",
    "python.linting.flake8Path": "/usr/local/py-utils/bin/flake8",
    "python.linting.mypyPath": "/usr/local/py-utils/bin/mypy",
    "python.linting.pycodestylePath": "/usr/local/py-utils/bin/pycodestyle",
    "python.linting.pydocstylePath": "/usr/local/py-utils/bin/pydocstyle",
    "python.linting.pylintPath": "/usr/local/py-utils/bin/pylint"
  },
  "extensions": ["ms-python.python", "ms-python.vscode-pylance"],
  "portsAttributes": {
    "9000": {
      "label": "Hello Remote World",
      "onAutoForward": "notify"
    }
  },
  "postCreateCommand": "pip3 install -r requirements.txt",
  "remoteUser": "vscode"
}
```

- `{"onAutoForward": "notify"}`を`{"onAutoForward": "openBrowser"}` に変更すると、ブラウザが立ち上がるようになる
- コンテナに影響する変更は、`Rebuild Container` コマンドを実行する必要がある

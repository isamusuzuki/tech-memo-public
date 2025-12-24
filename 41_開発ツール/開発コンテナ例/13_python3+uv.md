# 開発コンテナ例 python3+uv

作成日 2025/10/01、更新日 2025/12/10

## .devcontainer/devcontainer.json

```json
{
    "name": "python3+uv",
    "build": {
        "dockerfile": "./Dockerfile"
    },
    "customizations": {
        "vscode": {
            "extensions": [
                "ms-python.python",
                "charliermarsh.ruff",
                "njpwerner.autodocstring",
                "shardulm94.trailing-spaces",
                "oderwat.indent-rainbow",
                "mechatroner.rainbow-csv"
            ],
            "settings": {
                "autoDocstring.docstringFormat": "numpy",
                "[python]": {
                    "editor.defaultFormatter": "charliermarsh.ruff",
                    "editor.formatOnSave": true,
                    "editor.codeActionsOnSave": {
                        "source.fixAll": "explicit",
                        "source.organizeImports": "explicit"
                    }
                },
                "terminal.integrated.env.linux": {
                    "PYTHONPATH": "${containerWorkspaceFolder}"
                },
                "python.analysis.extraPaths": [
                    "${containerWorkspaceFolder}/",
                    "${containerWorkspaceFolder}/.venv/lib/python3.12/site-packages/"
                ],
                "python.analysis.typeCheckingMode": "standard"
            }
        }
    }
}
```

## .devcontainer/Dockerfile

```bash
FROM python:3.12-slim-trixie

# The installer requires curl (and certificates) to download the release archive
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    ca-certificates \
    git \
    openssh-client \
    fonts-ipaexfont-gothic \
    && rm -rf /var/lib/apt/lists/*

# Download the latest installer
ADD https://astral.sh/uv/install.sh /uv-installer.sh

# Run the installer then remove it
RUN sh /uv-installer.sh && rm /uv-installer.sh

# Ensure the installed binary is on the `PATH`
ENV PATH="/root/.local/bin/:$PATH"

# タイムゾーンをJSTに変更する
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
```

## Dockerイメージ調査

[python - Official Image | Docker Hub](https://hub.docker.com/_/python)

[python/3.12/slim-bookworm/Dockerfile](https://github.com/docker-library/python/blob/e4ab0fe5ef4df797ed09883becf983a56ab97eca/3.12/slim-bookworm/Dockerfile)

useraddはしていない => rootユーザーを使うのが正解らしい

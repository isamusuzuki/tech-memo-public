# Ruffを導入する

作成日 2025/06/18、更新日 2025/12/23

## 1. Ruffとは？

公式サイト（英語） => [Ruff](https://docs.astral.sh/ruff/)

> An extremely fast Python linter and code formatter, written in Rust.

Rustで書かれた、めちゃくちゃ速い、Pythonリンター兼コードフォーマッター

## 2. 開発コンテナでの設定

- インストールする機能拡張のリストからautopep8とisortを取り除き、ruffを追加する
- Pythonコードのデフォルトのフォーマッターをruffに設定する

```json
{
    "customizations": {
        "vscode": {
            "extensions": [
                "ms-python.python",
                "charliermarsh.ruff",
                // "ms-python.autopep8",
                // "ms-python.isort",
            ],
            "settings": {
                "[python]": {
                    "editor.defaultFormatter": "charliermarsh.ruff",
                    "editor.formatOnSave": true,
                    "editor.codeActionsOnSave": {
                        "source.fixAll": "explicit",
                        "source.organizeImports": "explicit"
                    }
                }
            }
        }
    }
}
```

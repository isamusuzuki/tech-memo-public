# WSL 再インストール

作成日 2025/01/28、更新日 2025/01/29

```bash
# インストール済みのディストリビューションの一覧を表示する
wsl --list -v

# インストール済みのUbuntuを削除する
wsl --unregister Ubuntu

# Ubuntuをインストールする
wsl --install -d Ubuntu

# Ubuntuをデフォルトに設定する
wsl --setdefault Ubuntu

# デフォルトのUbuntuを起動する
wsl
```

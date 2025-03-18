# winget コマンド

作成日 2025/03/18

winget はコマンドラインツール。インストールプログラムのダウンロードと起動までをコマンドラインで実行できる

PowerShell を起動する

```bash
winget --version
# v1.10.340

winget search volta
# 名前  ID          バージョン ソース
# ---------------------------------
# Volta Volta.Volta 2.0.2     winget

winget install Volta.Volta
```

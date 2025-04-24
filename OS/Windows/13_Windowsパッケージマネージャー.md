# Windowsパッケージマネージャー

作成日 2025/04/24

## 1. Windowsパッケージマネージャー

公式ガイド => [Windows パッケージ マネージャー](https://learn.microsoft.com/ja-jp/windows/package-manager/)

## 2. wingetコマンド

インストールプログラムのダウンロードと起動までをコマンドラインで実行できる

```bash
winget --version
# v1.10.340

winget search volta
# 名前  ID          バージョン ソース
# ---------------------------------
# Volta Volta.Volta 2.0.2     winget

winget install Volta.Volta
```

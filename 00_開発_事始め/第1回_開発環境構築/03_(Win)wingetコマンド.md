# wingetコマンド

作成日 2025/04/24、更新日 2026/01/06

## 1. 公式ドキュメントを読む

[WinGet | Microsoft Learn](https://learn.microsoft.com/ja-jp/windows/package-manager/)

## 2. wingetコマンドを使う

インストールプログラムのダウンロードと起動までをコマンドラインで実行できる

```bash
winget --version
# v1.12.350

winget search Git
# 名前 ID                 バージョン ソース
# ------------------------------------------
# Git  Git.Git            2.52.0     winget

winget install Git.Git

winget search DockerDesktop
# 名前                ID                       バージョン  ソース
# ----------------------------------------------------------------
# Docker Desktop      Docker.DockerDesktop     4.55.0      winget

winget install Docker.DockerDesktop

winget search GIMP
# 名前         ID                バージョン ソース
# -------------------------------------------------
# GIMP         GIMP.GIMP.3       3.0.6.1    winget

winget install GIMP.GIMP.3
```

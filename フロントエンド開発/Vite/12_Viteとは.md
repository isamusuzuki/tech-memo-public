# Viteとは

作成日 2025/09/09

## 1. 公式サイト（日本語）を読む

[次世代フロントエンドツール](https://ja.vite.dev/)

> Vite は、次世代の Web アプリケーションを支える超高速フロントエンドビルドツールです。

開発者体験の再定義

- 即時サーバー起動
- 超高速 HMR (Hot Module Replacement)
- 豊富な機能
- ビルドの最適化

ビルドのための共通基盤

- 柔軟なプラグインシステム
- 完全な型付けAPI
- ファーストクラスのSSRサポート
- 継続的なエコシステム統合

## 2. ユーザーガイド（日本語）を読む

[はじめに](https://ja.vite.dev/guide/)

## 3. Viteで新規アプリを作成する

### `22-bookworm` の開発コンテナを起動する

```bash
cat /etc/os-release
# PRETTY_NAME="Debian GNU/Linux 12 (bookworm)"

node --version
# v22.12.0

npm --version
# 10.9.0
```

### npm createコマンドを叩き、質問にいくつか答える

```bash
npm create vite@latest

# Project name:
# app

# Select a framework:
# Vue

# Select a variant:
# TypeScript

# Scafolding project in /workspaces/vuejs-vite-dojo/app...
# Done. Now run:
```

### アプリを起動する

```bash
cd app
npm install

tsc --version

npm run dev
```

# vite-vue-dojoリポジトリ紹介

作成日 2025/04/22、更新日 2025/04/26

[isamusuzuki/vite-vue-dojo: Vite + Vue 道場](https://github.com/isamusuzuki/vite-vue-dojo)

- Vite + Vue.js + TypeScript で開発したコードサンプル集

## 1. Viteとは

公式サイト（日本語） => [次世代フロントエンドツール](https://ja.vite.dev/)

- 即時サーバー起動
- 超高速 HMR (Hot Module Replacement)
- 豊富な機能拡張を提供する開発サーバー
- ビルドの最適化

ユーザーガイド（日本語） => [はじめに](https://ja.vite.dev/guide/)

## 2. Viteで新規アプリを作成する

### `22-bookworm` の開発コンテナを起動する

```bash
cat /etc/os-release
# PRETTY_NAME="Debian GNU/Linux 12 (bookworm)"

node --version
# v22.12.0

npm --version
# 10.9.0
```

### npmコマンドを叩いて、質問にいくつか答える

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

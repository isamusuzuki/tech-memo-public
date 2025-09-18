# Bun導入

作成日 2025/07/02、更新日 2025/07/04

## 1. Bunとは

公式サイト => [Bun — A fast all-in-one JavaScript runtime](https://bun.sh/)

Bunは、Node.js+npmとの100%互換を掲げてはいるものの、依存も利用もしていない

TypeScriptを直接走らせることができるところが気に入った

## 2. Windowsにインストールする

```bash
powershell -c "irm bun.sh/install.ps1 | iex"
# Bun 1.2.17 was installed successfully!
# The binary is located at C:\Users\{username}\.bun\bin\bun.exe
# To get started, restart your terminal/editor, then type "bun"
```

PowerShellを再起動してから

```bash
bun --version
# 1.2.17
```

## 3. 新規プロジェクトを開始する

```bash
mkdir new-project
cd new-project
bun init
# Select a project template

# ✓ Select a project template: Blank
#
# + .gitignore
# + index.ts
# + tsconfig.json (for editor autocomplete)
# + README.md
#
#　To get started, run:
#
#    bun run index.ts

bun run index.ts
# Hello via Bun!
```

自動生成されたファイル＆フォルダ構成

```text
--new-project/
    |--node_modules/
    |--.gitignore
    |--bun.lock
    |--index.ts
    |--package.json
    |--README.md
    `--tsconfig.json
```

## 4. よく使うコマンド

```bash
# package.jsonのscripts項目に書いてあるコマンドを実行する
bun run start

# パッケージをインストールする
bun add figlet
bun add -d @types/figlet

# パッケージをアンインストールする
bun remove figlet @types/figlet
```

## 5. `.env`ファイルの中身を環境変数に組み込む

[Environment variables – Runtime | Bun Docs](https://bun.sh/docs/runtime/env)

> Bun reads your .env files automatically and provides idiomatic ways to read and write your environment variables programmatically.

`.env`ファイルを用意しておけば勝手に読んでくれるようである

> Bun supports `--env-file` to override which specific `.env` file to load. You can use `--env-file` when running scripts in bun's runtime, or when running package.json scripts.

```bash
bun --env-file=.env.abc --env-file=.env.def run build
```

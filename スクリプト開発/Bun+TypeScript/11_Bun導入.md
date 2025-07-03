# Bun導入

作成日 2025/07/02

公式サイト => [Bun — A fast all-in-one JavaScript runtime](https://bun.sh/)

## 1. Windowsにインストールする

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

## 2. 新規プロジェクトを開始する

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
#To get started, run:
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

## 3. よく使うコマンド

```bash
# package.jsonのscripts項目に書いてあるコマンドを実行する
bun run start

# パッケージをインストールする
bun add figlet
bun add -d @types/figlet
```

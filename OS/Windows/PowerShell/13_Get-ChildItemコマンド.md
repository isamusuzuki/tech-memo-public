# Get-ChildItemコマンド

作成日 2025/04/11

## 1. 隠しファイルを表示する

```bash
Get-ChildItem -Force

ls -Force
# ls -a ではない
```

## 2. 指定ディレクトリ内のファイル名の一覧をファイルに書き出す

```bash
Get-ChildItem target-dir -Name > names.txt
```

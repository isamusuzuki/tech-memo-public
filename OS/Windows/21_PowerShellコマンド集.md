# PowerShell コマンド集

作成日 2025/01/29、更新日 2025/03/18

## 1. バージョン確認

```bash
$PSVersionTable
```

## 2. 環境変数の一覧表示

```bash
Get-ChildItem env:
# envという名前のドライブという扱い
```

## 3. （一時的な）環境変数の設定

```bash
$env:someValue = "test"
```

## 4. 隠しファイルを表示する

```bash
# ls -a ではない
ls -Force

Get-ChildItem -Force
```

## 5. ディレクトリを削除する

```bash
Remove-Item target-dir

# 強制的に削除する
Remove-Item target-dir -Force

# 再帰的に削除する
Remove-Item target-dir -Recurse
```

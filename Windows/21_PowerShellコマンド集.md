# PowerShell コマンド集

作成日 2025/01/29、更新日 2025/02/18

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

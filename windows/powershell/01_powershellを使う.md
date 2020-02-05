# Powershell を使う

作成日 2020/02/04

## 01. 環境・設定を確認する

```bash
# バージョンを確認する
$Host.Version
# => 5 1 18362 145

# 文字コードを確認する
$OutputEncoding.EncodingName
# => US_ASCII

# 実行ファイルのありかを知る
$PSHOME
# => C:\Windows\System32\WindowsPowerShell\v1.0
```

### 環境変数を確認・変更する

```bash
# 環境変数の一覧を取得する（名前がabc順に並ぶ）
get-childitem env:

# 変数名を指定することもできるが、存在しない場合はエラーになる
get-childitem env:NODE_ENV

# セッションの環境変数を設定する（ウィンドウを閉じればなくなる）
set-item env:NODE_ENV -value production
set-item env:NODE_ENV -value production;node ./aihara.js
```

## 02. ドライブやフォルダ、ファイルを操作する

```bash
# PSドライブの一覧を取得する
Get-PSDrive

# 新しいPSドライブを作成する
New-PSDrive -Name Z -PSProvider FileSystem -Root \\beatles\公開フォルダ -Persist

# PSドライブを削除する
Remove-PSDrive -Name Z
```

### アイテムを削除する

```bash
# パスで指定されたアイテム（ディレクトリ、ファイル、レジストリキー）を削除する
Remove-Item {path}

# フォルダを削除する
Remove-Item c:\test -Recurse -Force

# パスで指定されたアイテムの内容を削除する。内容を削除するだけでアイテム自体は削除しない
Clear-Item {path}

# パスで指定されたファイルの内容を削除する（ファイルだけに使う）
Clear-Content {path}
```

### 特定のファイルがなかったら作成しておく

```bash
$textfile = ".\temp\mail.txt"
if (-not (Test-Path $textfile)) {
  New-Item $textfile -ItemType file
}
```

### ファイル名の一覧をファイルに吐き出す

```bash
Get-ChildItem "C:\Users\{user-name}\Documents" -Recurse -Name > C:\Users\{user-name}\Desktop\filename.txt
```

### ディレクトリのサイズを一覧表示する

```bash
$startFolder = "C:\Users\{user-name}"

$colItems = (Get-ChildItem $startFolder | Measure-Object -property length -sum)
$startFolder + " -- " + "{0:N2}" -f ($colItems.sum / 1MB) + " MB"

$colItems = (Get-ChildItem $startFolder -recurse | Where-Object {$_.PSIsContainer -eq $True} | Sort-Object)
foreach ($i in $colItems)
{
    $subFolderItems = (Get-ChildItem $i.FullName | Measure-Object -property length -sum)
    $i.FullName + " -- " + "{0:N2}" -f ($colItems.sum / 1MB) + " MB"
}
```

### スクリプトファイルがある場所を知る

```bash
$datetimetext = Get-Date -Format yyyyMMdd_HHmmss

# ファイル名のフルパスから、ディレクトリ名を抜き出す
$directorypath = Split-Path $MyInvocation.MyCommand.Path

# ファイル名を繋げるときは、冒頭にバックスラッシュをつける
[String]$outputfile = $directoryPath + "\..\temp\$($datetimetext).txt"
```

## 03. Powershellから、様々なものを実行する

### 外部コマンドを実行する

```bash
Start-Process -FilePath "C:\ProgramData\chocolatey\bin\7z.exe" -ArgumentList "a .\temp\$($name).zip $($files) -p$($password)" -Wait
```

### vscode を起動する

vscode を起動した時に、特定のフォルダを開いた状態にする

~/work_memo.ps1

```bash
set-location 'Google ドライブ\work-memo'; code .
```

日本語が含まれているディレクトリで、きちんと遷移させるには、\
ps1 ファイルの文字コードを`UTF-8 with BOM`にして保存すればよい

### Python スクリプトを起動する

~/test.ps1

```bash
set-location 'andy'; python test.py >> ./temp/test.log
```

~/andy/test.py

```python
print('hello, world')
```

PowewrShellを開く

```bash
./test.ps1
```

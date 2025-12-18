# Get-ChildItemコマンド

作成日 2025/04/11

## 1. 隠しファイルを表示する

```bash
Get-ChildItem -Force

ls -Force
# ls -aではない
```

## 2. 環境変数の一覧表示

```bash
Get-ChildItem env:
# envという名前のドライブという扱い
```

## 3. ファイル名の一覧を表示する

```bash
# 指定ディレクトリ内のファイル名の一覧を表示する
# 指定なしは、Mode,LastWriteTime,Length,Nameを表示
Get-ChildItem C:\Users\user-a\project-a
# 表示項目を指定する
Get-ChildItem C:\Users\user-a\project-a -Name
# コンソールに表示せずにファイルに書き出す
Get-ChildItem C:\Users\user-a\project-a -Name > names.txt

# 指定ディレクトリ内のファイル名をサブフォルダの中まで調べて
# 拡張子が.mdのものだけに絞って、フルパスで、ファイルに書き出す
Get-ChildItem -Path C:\Users\user-a\project-a -Filter *.md -Recurse |
select-Object {$_.FullName} |
Out-File -FilePath temp\filenames.txt -Encoding utf8
```

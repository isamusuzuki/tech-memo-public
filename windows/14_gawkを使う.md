# gawk を使う

作成日 2019/07/01、更新日 2019/07/02

## 01. awk とは

テキストファイルの処理に特化したコマンドラインツール。UNIX 系 OS には標準で含まれている

MySQL のコマンドラインツールが、TSV（タブ区切り）データを吐き出す。Excel は、CSV（カンマ区切り）データを使いたい

awk を使って、その TSV データを CSV データに変換しようと目論んでいる

## 02. Windows 10 に gawk をインストールする

GNU が作成した awk のことを「gawk」という。これは Windows 版が用意されている

- ダウンロードサイト => [https://www.vector.co.jp/soft/win95/util/se376460.html](https://www.vector.co.jp/soft/win95/util/se376460.html)

- `gawk-mbcs-win32-20051223.zip`ファイル（455KB）をダウンロード
- 圧縮ファイルの中から、gawk.exe だけを抜き出す。いったんデスクトップにでも置く
- `C:\tools`フォルダを作成する。すでにそのフォルダがあれば、スキップ
- `C:\tools`フォルダへの PATH を通す。コントロールパネル ＞ 表示方法：小さいアイコン ＞ システム
- 左枠 ＞ システムの詳細設定 ＞ 「システムのプロパティ」ウィンドウが登場
- 詳細設定タブ ＞ 環境変数ボタン ＞ 「環境変数」ウィンドウが登場
- ユーザー環境変数（上コーナー）の変数の中から、Path を選んで ＞ 編集ボタン
- 「環境変数名の編集」ウィンドウが登場 ＞ `C:\tools`を追加して、OK ボタン
- さらに、OK ボタンを 2 回クリックして、すべてを閉じる
- `gawk.exe`を、`C:\tools`フォルダに移動させる

### awk のインストールの確認

- コマンドプロンプトを起動する
- 以下のコマンドを叩いて、バージョン番号が表示されることを確認する

```bash
gawk --version
#=> GNU Awk 3.1.5
```

## 02. gawk を使って TSV を CSV に変換する

用意したファイルは 6 つ

| No  | file             | contents   | encoding          |
| --- | ---------------- | ---------- | ----------------- |
| 1   | todofuken1.tsv   | 日本語あり | Shift JIS         |
| 2   | todofuken2.tsv   | 日本語あり | UTF-8（BOM なし） |
| 3   | todofuken3.tsv   | 日本語あり | UTF-8（BOM 付き） |
| 4   | prefectures1.tsv | 日本語なし | Shift JIS         |
| 5   | prefectures2.tsv | 日本語なし | UTF-8（BOM なし） |
| 6   | prefectures3.tsv | 日本語なし | UTF-8（BOM 付き） |

```bash
gawk "BEGIN { FS=\"\t\"; OFS=\",\" } {$1=$1; print}" todofuken1.tsv > todofuken1.csv
#=> Excelでそのまま開くことができた

gawk "BEGIN { FS=\"\t\"; OFS=\",\" } {$1=$1; print}" todofuken2.tsv > todofuken2.csv
#=> Excelで開いたら文字化けした

gawk "BEGIN { FS=\"\t\"; OFS=\",\" } {$1=$1; print}" todofuken3.tsv > todofuken3.csv
#=> Excelでそのまま開くことができた

gawk "BEGIN { FS=\"\t\"; OFS=\",\" } {$1=$1; print}" prefectures1.tsv > prefectures1.csv
gawk "BEGIN { FS=\"\t\"; OFS=\",\" } {$1=$1; print}" prefectures2.tsv > prefectures2.csv
gawk "BEGIN { FS=\"\t\"; OFS=\",\" } {$1=$1; print}" prefectures3.tsv > prefectures3.csv
#=> Excelでそのまま開くことができた
```

## 03. 同僚の資料に書いてあることを再現する

### MySQL コマンドラインツールのインストール

- Chocolatey を使う
- Windows PowerShell を管理者として実行する
- `choco install mysql-cli -y`
- Windows を再起動する

### MySQL コマンドラインツールのインストールの確認

- コマンドプロンプトを起動する
- 以下のコマンドを叩いて、バージョン番号が表示されることを確認する

```bash
mysql --version
#=> C:\ProgramData\chocolatey\lib\mysql-cli\tools\mysql.exe
#=> Ver 8.0.11 for Win64 on x86_64 (MySQL Community Server - GPL)
```

### データベースへの接続を確認する

```bash
mysql -u user -p -h 192.168.2.19 -D db
#=> Enter password:
#=> mysql>

#=> 以下はmysqlのシェル内
#=> SELECT COUNT(*) FROM db.cm_item_attribute where attribute_type = 2;
#=> 43667
#=> \q
```

### 再現を開始する

- コマンドプロンプトを起動する
- `select_attribute.sql`ファイルを作成する

```sql
SELECT * FROM cm_item_attribute where attribute_type = 2;
```

- コマンドを実行する

```bash
cd ~\Desktop
mysql -u user -pPASSWORD -h 192.168.2.19 \
-D db < select_attribute.sql > out.txt
#=> 43667行のテキストデータが出来上がった。これは日本語があるシフトJISだった

#パイプでつないでみる
mysql -u user -pPASSWORD -h 192.168.2.19 \
-D db < select_attribute.sql | gawk \
"BEGIN { FS=\"\t\"; OFS=\",\" } {$1=$1; print}" > out.csv
#=> うまくいった
```

### 新たな疑問が浮上する

attribute_code がハイフン始まりなので、エクセルに読ませると、自動的にイコールが追加されて、「#NAME?」になる

=> とりあえず、新たな疑問は、問題ではないとのこと

### データの中にカンマがあった

そこだけ列数がズレる問題が発生 => それを解決するためには、もっとちゃんとした AWK が必要

- MySQL コマンドラインツールでは、いったんファイル (out.txt) に保存する
- 以下をコマンドを実行する（bat ファイルに保存して実行してもよい）

```bash
gawk -f "tsv2csv.awk" out.txt > file.csv
```

tsv2csv.awk

```bash
BEGIN { FS="\t"; OFS="," } {
  rebuilt=0
  for(i=1; i<=NF; ++i) {
    if ($i ~ /,/ && $i !~ /^".*"$/) {
      gsub("\"", "\"\"", $i)
      $i = "\"" $i "\""
      rebuilt=1
    }
  }
  if (!rebuilt) { $1=$1 }
  print
}
```

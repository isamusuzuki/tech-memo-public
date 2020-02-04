# MySQL にアクセスする

作成日 2020/02/04

## 01. Connector/Net をインストールする

[MySQL :: Download Connector/Net](https://dev.mysql.com/downloads/connector/net/)

## 02. MySQLからデータを読む

```bash
# MySQL DBへの接続を準備
[void][System.Reflection.Assembly]::LoadFile("C:\Program Files (x86)\MySQL\Connector.NET 6.9\Assemblies\v4.5\MySql.Data.dll")
$connectionString = "userid=dbuser;password=dbpasswd;Host=db.xxxxx.ap-northeast-1.rds.amazonaws.com;database=db;"
$queryString = "SELECT * FROM coupons WHERE groupName = 'test' AND isUsed = 0 ORDER BY createdAt, coupon;"

# MySQL DBへの接続を実行
$connection = New-Object MySql.Data.MysqlClient.MySqlConnection($connectionString)
$command = New-Object MySql.Data.MySqlClient.MySqlCommand($queryString, $connection)
$connection.Open()
$reader = $command.ExecuteReader()

# コンソールへ出力
while ($reader.Read())
{
    Write-Output("{0}" -f $reader["coupon"])
}

# MySQL DBへの接続をクローズ
$command.Dispose()
$connection.Close()
$connection.Dispose()
```

## 02. 抽出データをファイルに出力する

```bash
# MySQL DBへの接続を準備
[void][System.Reflection.Assembly]::LoadFile("C:\Program Files (x86)\MySQL\Connector.NET 6.9\Assemblies\v4.5\MySql.Data.dll")
$connectionString = "userid=dbuser;password=dbpasswd;Host=db.xxxxx.ap-northeast-1.rds.amazonaws.com;database=db;"
$queryString = "SELECT * FROM entries WHERE groupName != 'test' AND  entryStatus = '06_order_done' AND shippingStatus is null"

# 出力ファイルを準備
$datetimetext = Get-Date -Format yyyyMMdd_HHmmss
$directorypath = Split-Path $MyInvocation.MyCommand.Path
[String]$outputfile = $directorypath + "\..\temp\$($datetimetext).txt"

# MySQL DBへの接続を実行
$connection = New-Object MySql.Data.MysqlClient.MySqlConnection($connectionString)
$command = New-Object MySql.Data.MySqlClient.MySqlCommand($queryString, $connection)
$connection.Open()
$r = $command.ExecuteReader()

# 出力ファイルのストリームを開く
$streamwriter = New-Object System.IO.StreamWriter($outputfile, $false, [Text.Encoding]::GetEncoding("UTF-8"))

while ($r.Read())
{
    $streamwriter.WriteLine(
        "{0}`t{1}`t{2}`t{3} {4}`t{5} {6}`t{7}`t{8}`t{9}{10}`t{11} {12}`t{13}`t{14}`t{15}`t{16}`t{17}`t{18}{19}`t{20}`t{21}`t{22}`t{23}",
        $r['id'], $r['coupon'], $r['groupName'], $r['kanjiLastname'], $r['kanjiFirstname'],
        $r['romanLastname'], $r['romanFirstname'], $r['printZipcode'], $r['printPrefecture'],
        $r['printAddress1'], $r['printAddress2'], $r['printGroup1'], $r['printGroup2'],
        $r['printEmail'], $r['printPhone'], $r['printCell'],
        $r['shippingZipcode'], $r['shippingPrefecture'], $r['shippingAddress1'], $r['shippingAddress2'],
        $r['shippingFullname'], $r['shippingPhone'], $r['shippingEmail'], $r['updatedAt']
    )
}

# 出力ファイルをクローズ
$streamwriter.Close()

# MySQL DBへの接続をクローズ
$command.Dispose()
$connection.Close()
$connection.Dispose()

Write-Output "file created."
```

## 03. データを更新する

```bash
# MySQL DBへの接続を準備
[void][System.Reflection.Assembly]::LoadFile("C:\Program Files (x86)\MySQL\Connector.NET 6.9\Assemblies\v4.5\MySql.Data.dll")
$connectionString = "userid=dbuser;password=dbpasswd;Host=db.xxxxx.ap-northeast-1.rds.amazonaws.com;database=db;"
$queryString = "UPDATE entries SET kanjiLastname = '*****' , kanjiFirstname = '*****' , romanLastname = '*****'
, romanFirstname = '*****' , printZipcode = '*****' , printPrefecture = '*****' , printAddress1 = '*****'  , printAddress2= '*****'
, printGroup1 = '*****' , printGroup2 = '*****' , printEmail = '*****' , printPhone = '*****' , printCell = '*****'
, shippingZipcode = '*****' , shippingPrefecture = '*****' , shippingAddress1 = '*****' , shippingAddress2 = '*****'
, shippingFullname = '*****' , shippingPhone = '*****' , shippingEmail = '*****'
, shippingStatus = 'copied' WHERE groupName != 'test' AND  entryStatus = '06_order_done'"

# MySQL DBへの接続を実行
$connection = New-Object MySql.Data.MysqlClient.MySqlConnection($connectionString)
$command = New-Object MySql.Data.MySqlClient.MySqlCommand($queryString, $connection)
$connection.Open()
$command.ExecuteNonQuery() | Out-Null

# MySQL DBへの接続をクローズ
$command.Dispose()
$connection.Close()
$connection.Dispose()

Write-Output "DB updated."
```

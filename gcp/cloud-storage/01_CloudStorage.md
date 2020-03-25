# Cloud Storage

作成日 2020/03/25

公式トップ => [Cloud Storage: オブジェクト ストレージ  \|  Google Cloud](https://cloud.google.com/storage/)

ドキュメントトップ => [Cloud Storage ドキュメント  \|  Google Cloud](https://cloud.google.com/storage/docs)

## 01. 基本コンセプト

### ストレージクラスの種類

- Standard .. optimized for performance and high frequency access
- Nearline ... for data accessed less than once a month
- Coldline ... for data accessed less than once a quarter
- Archive ... for data accessed less than once a year

### ストレージクラスの説明

すべてのストレージクラスには以下が適用される

- 最小オブジェクト サイズのない無制限ストレージ
- 低レイテンシ（最初のバイトの転送時間は一般的に数十ミリ秒）
- 高い耐久性（99.999999999% の年間耐久性）
- Cloud Storage の機能、セキュリティ、ツール、API による一貫したエクスペリエンス

### ロケーションタイプの種類

- Regional ... stored in a specific region with replication across availability zones in that region
- Dual-region ... replicated across a specific pair of region
- Multi-region ... dstributed redundantly across US, EU or Asia

## 02. 料金体系

[Cloud Storage の料金  \|  Google Cloud](https://cloud.google.com/storage/pricing)

### 料金は次のコンポーネントに分類される

- データストレージ ... バケットへのデータの格納
- ネットワーク使用量 ... バケット内のデータのアクセスと移動
- オペレーション使用量 ... Cloud Storage 内でのオペレーションの実行
- 取得料金と早期削除料金 ... Nearline Storage, Coldline Storage にのみ適用

#### データストレージ（東京リージョン）

- Standard Storage ... \$0.023 GB/month
- Nearline Storage ... \$0.016 GB/month
- Coldline Storage ... \$0.006 GB/month

#### ネットワーク使用量（東京リージョン）

- 下り ... \$0.14 GB
- 上り ... 無料

#### オペレーション使用量

- クラス A オペレーション ... $0.05-$0.10 / 1 万オペレーションあたり
- クラス B オペレーション ... $0.004-$0.05 / 1 万オペレーションあたり

Standard よりも Nearline、Nearline よりも Coldline のほうが高い

#### 取得と早期削除

Nearline Storage と Coldline Storage は、取得料金が追加される

- Nearline Storage データ取得 ... \$0.01/GB
- Coldline Storage データ取得 ... \$0.05/GB

最小保存期間が適用され、最小保存期間に達する前に削除しても、\
最小期間保存された場合と同じ料金が請求される

- Nearline Storage 最小保存期間 ... 30 日
- Coldline Storage 最小保存期間 ... 90 日

## 03. ストレージクラスの変更の仕方

[オブジェクトのライフサイクル管理  \|  Cloud Storage  \|  Google Cloud](https://cloud.google.com/storage/docs/lifecycle)

### オブジェクトのライフサイクル管理機能とは

この機能を使って、以下のことができる

- オブジェクトの有効期間（TTL）の設定
- 古いバージョンのアーカイブ
- ストレージ クラスのダウングレード

バケット内に現在あるオブジェクトと今後追加されるオブジェクトに適用されるルールを構成できる\
オブジェクトがいずれかのルールの条件に一致すると、\
Cloud Storage はオブジェクトに指定された操作を自動的に実行する

- ライフサイクルの条件 => Age, CreatedBefore, IsLive, MatchesStorageClass, NumberOfNewerVersion
- ライフサイクルの操作 => Delete, SetStorageClass

バージョニングされていないバケットのオブジェクトは、`IsLive=true` とみなされる

# Compute Engine

作成日 2019/11/15

## 01. Compute Engine とは

IaaS = Infrastructure as a Service

公式トップ => [https://cloud.google.com/compute/?hl=ja)](https://cloud.google.com/compute/?hl=ja)

ドキュメント => [https://cloud.google.com/compute/docs/?hl=ja](https://cloud.google.com/compute/docs/?hl=ja)

### Compute Engine の料金

[https://cloud.google.com/compute/all-pricing?hl=ja](https://cloud.google.com/compute/all-pricing?hl=ja)

g1-small(0.5 vCPU, 1.70GB), 東京リージョン\
=> 1 ヶ月あたり\$16.45, 1 ドル 110 円換算で 1,810 円

## 02. GCP コンソールで VM インスタンスを立てる

左枠 ＞ Compute Engine ＞ VM インスタンス\
右枠 ＞ 作成ボタン\
「インスタンスの作成」ページ ＞ 新規 VM インスタンス

| Key                        | Value                                                     |
| -------------------------- | --------------------------------------------------------- |
| 名前                       | 適当につける                                              |
| リージョン                 | asia-northeast1                                           |
| ゾーン                     | asia-northeast1-b                                         |
| マシンファミリー           | 汎用                                                      |
| マシンタイプ               | g1-small (1 vCPU, 1.7GB メモリ)                           |
| コンテナ                   | コンテナイメージをデプロイ、チェックオフ                  |
| ブートディスク OS イメージ | Ubuntu 18.04 LTS Minimal                                  |
| ブートディスクの種類       | 標準の永続ディスク 10GB                                   |
| サービスアカウント         | Compute Engine default service account                    |
| アクセススコープ           | デフォルトのアクセス権を許可                              |
| ファイアウォール           | HTTP トラフィックを許可する, HTTPS トラフィックを許可する |

作成ボタン

### 立ち上がった VM インスタンスに SSH 接続する

参照先 => [インスタンスへの接続](https://cloud.google.com/compute/docs/instances/connecting-to-instance)

#### GCP コンソールから、ブラウザ経由で SSH 接続が可能

VM インスタンス ＞ 該当行の「SSH」メニュー ＞ 「ブラウザウィンドウで開く」

```bash
cat /etc/os-release
# => NAME="Ubuntu"
# => VERSION="18.04.3 LTS (Bionic Beaver)"

sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
```

#### SSH 認証鍵を取得する

```bash
gcloud compute ssh --project PROJECT_ID --zone ZONE INSTANCE_NAME
```

1 回目は時間がかかる\
2 回目はもう少し早い\
いずれにしろ、PuTTY が立ち上がり、その中で利用する\
gcloud を叩いたターミナルで SSH 接続できるわけではない

### Ubuntu 18.04 LTS Minimal を選択したときの注意事項

vim も less も入っていない。自分でインストールする

```bash
sudo apt install vim less
```

## 03. 固定 IP アドレスを割り当てる

左枠 ＞ VPC ネットワーク ＞ 外部 IP アドレス\
右上「静的アドレスを予約」ボタン ＞ 「静的アドレスを予約」ページ

| Key                      | Value                   |
| ------------------------ | ----------------------- |
| 名前                     | 適当につける            |
| ネットワークサービス階層 | プレミアム              |
| IP バージョン            | IPv4                    |
| タイプ                   | リージョン              |
| リージョン               | asia-northeast1（東京） |
| 接続先                   | VM インスタンスの名前   |

予約ボタン

=> 固定 IP アドレスが割り当たった

## 04. MySQL のポートを空ける

左枠 ＞ VPC ネットワーク ＞ ファイアウォールルール\
右枠 ＞ 右上の「ファイアウォールルールを作成」をクリック\
「ファイアウォールルールを作成」ページ

| Key                      | Value                                  |
| ------------------------ | -------------------------------------- |
| 名前                     | default-allow-mysql                    |
| ログ                     | オフ                                   |
| ネットワーク             | default                                |
| 優先度                   | 1000                                   |
| トラフィックの方向       | 上り                                   |
| 一致したときのアクション | 許可                                   |
| ターゲット               | ネットワーク上のすべてのインスタンス   |
| ソースフィルタ           | IP 範囲                                |
| ソース IP の範囲         | 0.0.0.0/0                              |
| 2 番目のソースフィルタ   | なし                                   |
| プロトコルとポート       | 指定したプロトコルとポート（TCP 3306） |

作成ボタン

左枠 ＞ Compute Engine ＞ VM インスタンス\
「VM インスタンス」ページ ＞ 該当のインスタンス\
行右端の縦 3 点をクリック ＞ ネットワークの詳細の表示\
「ネットワークインターフェースの詳細」ページ\
ファイアウォールルールに、default-allow-mysql があることを確認する

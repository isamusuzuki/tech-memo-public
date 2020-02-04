---
tags: gcp
---

# Compute Engine

作成日 2019/11/15

## 01. Compute Engineとは

IaaS = Infrastructure as a Service

公式トップ => [https://cloud.google.com/compute/?hl=ja)](https://cloud.google.com/compute/?hl=ja)

ドキュメント => [https://cloud.google.com/compute/docs/?hl=ja](https://cloud.google.com/compute/docs/?hl=ja)

### Compute Engineの料金

[https://cloud.google.com/compute/all-pricing?hl=ja](https://cloud.google.com/compute/all-pricing?hl=ja)

g1-small(0.5 vCPU, 1.70GB), 東京リージョン\
=> 1ヶ月あたり$16.45, 1ドル110円換算で1,810円

## 02. GCPコンソールでVMインスタンスを立てる

左枠 ＞ Compute Engine ＞ VMインスタンス\
右枠 ＞ 作成ボタン\
「インスタンスの作成」ページ ＞ 新規VMインスタンス

Key | Value
----|------
名前 | 適当につける
リージョン | asia-northeast1
ゾーン | asia-northeast1-b
マシンファミリー | 汎用
マシンタイプ | g1-small (1 vCPU, 1.7GB メモリ)
コンテナ | コンテナイメージをデプロイ、チェックオフ
ブートディスク OSイメージ | Ubuntu 18.04 LTS Minimal
ブートディスクの種類 | 標準の永続ディスク 10GB
サービスアカウント | Compute Engine default service account
アクセススコープ   | デフォルトのアクセス権を許可
ファイアウォール   | HTTPトラフィックを許可する, HTTPSトラフィックを許可する

作成ボタン

### 立ち上がったVMインスタンスにSSH接続する

参照先 => [インスタンスへの接続](https://cloud.google.com/compute/docs/instances/connecting-to-instance)

#### GCPコンソールから、ブラウザ経由でSSH接続が可能

VMインスタンス ＞ 該当行の「SSH」メニュー ＞ 「ブラウザウィンドウで開く」

```bash=
cat /etc/os-release
# => NAME="Ubuntu"
# => VERSION="18.04.3 LTS (Bionic Beaver)"

sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
```

#### SSH認証鍵を取得する

```bash=
gcloud compute ssh --project PROJECT_ID\
  --zone ZONE\ INSTANCE_NAME
```

1回目は時間がかかる\
2回目はもう少し早い\
いずれにしろ、PuTTYが立ち上がり、その中で利用する\
gcloudを叩いたターミナルでSSH継続できるわけではない

### Ubuntu 18.04 LTS Minimalを選択したときの注意事項

vimもlessも入っていない。自分でインストールする

```bash=
sudo apt install vim less
```

## 03. 固定IPアドレスを割り当てる

左枠 ＞ VPCネットワーク ＞ 外部IPアドレス\
右上「静的アドレスを予約」ボタン ＞ 「静的アドレスを予約」ページ

Key | Value
----|------
名前 | 適当につける
ネットワークサービス階層 | プレミアム
IPバージョン | IPv4
タイプ | リージョン
リージョン | asia-northeast1（東京）
接続先 | VMインスタンスの名前

予約ボタン

=> 固定IPアドレスが割り当たった

## 04. MySQLのポートを空ける

左枠 ＞ VPCネットワーク ＞ ファイアウォールルール\
右枠 ＞ 右上の「ファイアウォールルールを作成」をクリック\
「ファイアウォールルールを作成」ページ

Key | Value
----|------
名前 | default-allow-mysql
ログ | オフ
ネットワーク | default
優先度 | 1000
トラフィックの方向 | 上り
一致したときのアクション | 許可
ターゲット | ネットワーク上のすべてのインスタンス
ソースフィルタ | IP範囲
ソースIPの範囲 | 0.0.0.0/0
2番目のソースフィルタ | なし
プロトコルとポート | 指定したプロトコルとポート（TCP 3306）

作成ボタン

左枠 ＞ Compute Engine ＞ VMインスタンス\
「VMインスタンス」ページ ＞ 該当のインスタンス\
行右端の縦3点をクリック ＞ ネットワークの詳細の表示\
「ネットワークインターフェースの詳細」ページ\
ファイアウォールルールに、default-allow-mysqlがあることを確認する

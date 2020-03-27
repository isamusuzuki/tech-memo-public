# Mermaid で図を作成する

作成日 2019/05/29、更新日 2019/12/04

## 01. Mermaid とは？

作図のためのスクリプト言語で、「フローチャート」「シーケンス図」「ガントチャート」の 3 つに特化している

```text
graph LR
A{チョキ} --> B((パー))
B --> C[グー]
C --> A
```

![mermaid-janken.png](https://imgur.com/ic81BTm.png)

公式サイト => [https://mermaidjs.github.io/](https://mermaidjs.github.io/)

## 02. Mermaid の文法

### 冒頭の宣言

- `graph TB` ... 上から下に描画する
- `graph LR` ... 左から右に描画する

### 要素をつくる

- `A[apple]` ... 四角形の要素をつくる
- `A(apple)` ... 角丸な四角形の要素をつくる
- `A{apple}` ... 菱形の要素をつくる
- `A((apple))` ... 円形の要素をつくる

### 要素と要素を結ぶ

- `A---B` ... A と B を直線で結ぶ
- `A-.-B` ... A と B を破線で結ぶ
- `A===B` ... A と B を太線で結ぶ
- `A-->B` ... A と B を直線矢印で結ぶ
- `A-.->B` ... A と B を破線矢印で結ぶ
- `A==>B` ... A と B を太線矢印で結ぶ
- `A-->|comment|B` ... 結び線にコメントを入れる

### その他

- `subgraph comment` ... 背景を描き始める
- `end` ... 背景を描き終わる
- `;` ... 一行を分けるときに使う、文末は不要

## 03. Mermaid Live Editor を使う

オンラインで図を作成し、スクリーンショットを撮って、PNG 画像にする

(1) [https://mermaidjs.github.io/mermaid-live-editor](https://mermaidjs.github.io/mermaid-live-editor)

(2) Mermaid configuration テキストボックスに以下をコピペする

```json
{
  "theme": "forest",
  "themeCSS": "{font-size: 16px;}"
}
```

(4) Code テキストボックスにコードを書く

(5) `Window+Shift+S`でスクリーンショットを撮る

## 04. Mermaid 作品集

### API

```text
graph LR
c(クライアント)-->|リクエスト|s(サーバー)
s-->|レスポンス|c
```

![vuejs-api.png](https://imgur.com/N5285pG.png)

### Vue.js Vuex

```text
graph LR
vc(Vueコンポーネント)-->|Dispatch|a(( Actions ))
a-->|非同期通信|ba[サーバーAPI]
ba-->a
a-->|Commit|m((Mutations))
m-->|更新|s((State))
vc-->|監視/変化|s
```

![vuejs-vuex.png](https://imgur.com/kbJlg2g.png)

### Vue.js State

```text
graph LR
b1((ボタン))-->|発射|p1{処理}
p1-->|更新|s1[状態]
t2(テーブル)-->|監視/変化|s2[状態]
m2(メッセージ)-->|監視/変化|s2[状態]
b2((ボタン))-->|監視/変化|s2[状態]
```

![vuejs-state.png](https://imgur.com/gGeXjk4.png)

### UI

```text
graph TB
subgraph UI
b1((ボタン1))
b2((ボタン2))
t(テーブル)
m(メッセージ)
end
b1-->p1{処理1}
b2-->p2{処理2}
p1-->t
p2-->t
p1-->m
p2-->m
p2-->b2
```

![vuejs-ui.png](https://imgur.com/CjnWSRB.png)

### Vue.js diagram

```text
graph TB
subgraph Vuex
m1(モジュール1)
m2(モジュール2)
end
subgraph Vueコンポーネント
p1(親コンポーネント)---c1(子コンポーネント)
p1---c2(子コンポーネント)
p1---c3(子コンポーネント)
p1---vr[Vue Router]
vr---pc1(ページコンポーネント)
vr---pc2(ページコンポーネント)
vr---pc3(ページコンポーネント)
end
```

![vuejs-vue-diagram.png](https://imgur.com/pj3ny5E.png)

### SSL Handshake

```text
sequenceDiagram
participant C as クライアント
participant S as サーバー
C->>S:接続要求
S->>C:サーバ証明書w/公開鍵を送付
C->>C:共通鍵を生成、公開鍵で暗号化
C->>S:暗号化した共通鍵を送付
S->>S:秘密鍵で共通鍵を復号
C->>S:共通鍵で暗号化したメッセージを送信
S->>C:共通鍵で暗号化したレスポンスを送信
```

![ssl-handshake.png](https://imgur.com/KcdD0ZF.png)

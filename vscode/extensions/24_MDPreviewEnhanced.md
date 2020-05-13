# Markdown Preview Enhanced

作成日 2019/02/08

公式トップ => [https://shd101wyy.github.io/markdown-preview-enhanced/](https://shd101wyy.github.io/markdown-preview-enhanced/)

## Markdown ファイルをプレビューする

- `ctrl+k,v`は、サイドバイサイドでプレビューを開く（かな漢字変換がオフになっていること）
  - プレビューを開いた後は、右クリックが使える
- `ctrl+shift+v`は、ひきつづき、純正のプレビューが別タブで表示される
- `ctrl+shift+p`でコマンドパレットを開き、`Markdown Preview Enhanced`を検索すると、たくさんコマンドが用意されている

## Emoji や Font Awesome が使える

```text
:smile:
:fa-user:
:fa-car:
```

:smile:
:fa-user:
:fa-car:

## Markdown 内の作図機能

[Diagrams \- Markdown Preview Enhanced](https://shd101wyy.github.io/markdown-preview-enhanced/#/diagrams)

> Markdown Preview Enhanced supports rendering flow charts, sequence diagrams, mermaid, PlantUML, WaveDrom, GraphViz, Vega & Vega-lite, Ditaa diagrams.

Mermaid が使えるのはとても便利

```mermaid
graph LR
    A{チョキ} --> B((パー))
    B --> C[グー]
    C --> A
```

[Vega-lite](https://vega.github.io/vega-lite/)を使ってグラフも作成できる

```vega-lite
{
    "description": "A simple bar chart with embedded data.",
    "data": {
        "values": [
            {"month": "2018/01", "sales": 20},
            {"month": "2018/02", "sales": 55},
            {"month": "2018/03", "sales": 43}
        ]
    },
    "mark": "bar",
    "encoding": {
        "x": {"field": "month", "type": "ordinal"},
        "y": {"field": "sales", "type": "quantitative"}
    },
    "width": 300,
    "height": 300
}
```

## PDF ファイルを作成する

**問題発生** ブラウザで開いてから印刷しようとすると、タイトルの頭の一文字目が欠けていることが判明した

[Puppeteer Export](https://shd101wyy.github.io/markdown-preview-enhanced/#/puppeteer)を試すことにした

### Puppeteer をローカルインストールする

Puppeteer の公式サイト => [https://developers.google.com/web/tools/puppeteer/](https://developers.google.com/web/tools/puppeteer/)

Git Bash を起動する

```bash
npm install -g puppeteer
npm list -g --depth=0
#=> C:\Users\exeo\AppData\Roaming\npm
#=> `-- puppeteer@1.12.2
```

`ctrl+v,k`を叩いて、サイドバイサイドで MDPE を表示する

Markdown ファイルの行頭にフロントマッターをとりつける

```text
---
puppeteer:
  landscape: false
  format: "A4"
---
```

### 印刷フォントをメイリオに変更する方法

- `ctrl+shift+p`を叩いてコマンドパレットを表示する
- `Markdown Preview Enhanced: Customize CSS`を見つける
- `Style.css`が登場するので、以下を追加する

```css
.markdown-preview.markdown-preview {
  @media print {
    font-family: "メイリオ";
  }
}
```

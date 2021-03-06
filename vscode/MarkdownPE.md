# Markdown Preview Enhanced

作成日 2019/02/08、更新日 2020/05/16

## 01. Markdown ファイルをプレビューする

- `ctrl+k,v`は、サイドバイサイドでプレビューを開く（かな漢字変換がオフになっていること）
  - プレビューを開いた後は、右クリックが使える
- `ctrl+shift+v`は、ひきつづき、純正のプレビューが別タブで表示される
- `ctrl+shift+p`でコマンドパレットを開き、`Markdown Preview Enhanced`を検索すると、たくさんコマンドが用意されている

## 02. Emoji や Font Awesome が使える

```text
:smile:
:fa-user:
:fa-car:
```

:smile:
:fa-user:
:fa-car:

## 03. Markdown 内の作図機能

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

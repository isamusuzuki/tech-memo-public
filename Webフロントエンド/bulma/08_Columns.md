# Columns を使う

作成日 2020/11/16

## 01. Columns とは

カラムをレスポンシブに配置する

[Columns \| Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/documentation/columns/)

```html
<div class="columns">
  <div class="column">First column</div>
  <div class="column">Second column</div>
  <div class="column">Third column</div>
  <div class="column">Fourth column</div>
</div>
```

## 02. Column の応用

`is-multiline`を指定することで、カラムの幅指定に基づき 1 行が構成されるようになる

[Column options \| Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/documentation/columns/options/)

カラムの幅指定　=> `is-three-quaters`, `is-two-thirds`, `is-half`, `is-one-third`, `is-one-quater`

```html
<div class="columns is-multiline">
  <div class="column is-half">
    <code>is-half</code>
  </div>
  <div class="column is-one-quarter">
    <code>is-one-quarter</code>
  </div>
  <div class="column is-one-quarter">
    <code>is-one-quarter</code>
  </div>
  <div class="column is-one-quarter">
    <code>is-one-quarter</code>
  </div>
  <!-- カラムの幅指定がないと残った幅を全て消化するように調整される -->
  <div class="column">Auto</div>
</div>
```

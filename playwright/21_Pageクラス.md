# Page クラス

作成日 2021/10/01

[Page \| Playwright](https://playwright.dev/python/docs/api/class-page)

## 01. 入力する

### `page.type(selector, text, **kwargs)` メソッド

引数解説

- selector ... 条件に沿うものが複数あった場合は最初の要素が使われる
- text ... 入力したい文字列

## 02. 何かを待つ

Page クラスには、以下の 5 つの待つメソッドがある

- `page.wait_for_event(event, **kwargs)`
- `page.wait_for_function(expression, **kwargs)`
- `page.wait_for_load_state(**kwargs)`
- `page.wait_for_selector(selector, **kwargs)`
- `page.wait_for_timeout(timeout)`

### `Page.wait_for_load_state()` メソッド

引数には、以下の 3 つを設定できる

- `load` ... load イベントが発射されるまで待つ（デフォルト）
- `documentloaded` ... DOMContentLoaded イベントが発射されるまで待つ
- `networkidle` ... ネットワーク接続がなくなってから少なくとも 500ms 待つ

`load`と`documentloaded`のどちらが先にくるかは、状況による模様。一概には言い切れない。デフォルトのまま、引数なしで使ってよいと思う

## 03. リストから選択する

- `page.select_option(selector, **kwargs)` メソッド

```python
page.select_option('select#colors, 'blue')
page.select_option('select#colors, label='blue')
page.select_option('select#colors, value=['red', 'green', 'blue'])
```

## 04. ページからHTML要素を抽出する

- `page.query_selector(selector, **kwargs)` メソッド
- `page.query_selector_all(selector)` メソッド

## 05. ページでJavaScriptコードを評価する

- `page.evaluate(expression, **kwargs)` メソッド

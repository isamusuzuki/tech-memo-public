# リストから選択する

作成日 2021/03/17

## Page クラス

[Page \| Playwright](https://playwright.dev/python/docs/api/class-page)

## `page.select_option(selector, **kwargs)` メソッド

```python
page.select_option('select#colors, 'blue')
page.select_option('select#colors, label='blue')
page.select_option('select#colors, value=['red', 'green', 'blue'])
```

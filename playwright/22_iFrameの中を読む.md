# iFrame の中を読む

作成日 2022/01/23

iFrame の外側と内側は完全な別世界となっている。連続した XPath で検索することはできない

## page の中の iframe を探す

- `page.frame(**kwargs)` returns `<NoneType|Frame>`
- `page.frames` returns `<List[Frame]>`

[https://playwright.dev/python/docs/api/class-page](https://playwright.dev/python/docs/api/class-page)

## iFrame の中の HTML 文字列を取得する

- `frame.content()` returns `<str>`

[https://playwright.dev/python/docs/api/class-frame](https://playwright.dev/python/docs/api/class-frame)

## 参照サイト

[https://thompson-jonm.medium.com/handling-iframes-using-python-and-playwright-da46d1c64196](https://thompson-jonm.medium.com/handling-iframes-using-python-and-playwright-da46d1c64196)

[https://stackoverflow.com/questions/62596046/how-to-use-a-selector-finding-an-frame-iframe-using-playwright](https://stackoverflow.com/questions/62596046/how-to-use-a-selector-finding-an-frame-iframe-using-playwright)

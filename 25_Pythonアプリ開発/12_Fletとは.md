# Fletとは

作成日 2025/11/26

## 1. 公式サイト（英語）を読む

[Build multi-platform apps in Python powered by Flutter | Flet](https://flet.dev/)

Flutterで強化されたPythonでマルチ・プラットフォーム・アプリを作る

Fletを使うと、開発者はPythonで、リアルタイムWebやデスクトップアプリを簡単に作れるようになる

フロントエンドの経験は必要ない

## 2. 公式ドキュメント（英語）を読む

[Introduction | Flet](https://flet.dev/docs)

### サンプルコード

counter.py

```python
import flet as ft

def main(page: ft.Page):
    page.title = "Flet counter example"
    page.vertical_alignment = ft.MainAxisAlignment.CENTER

    txt_number = ft.TextField(value="0", text_align=ft.TextAlign.RIGHT, width=100)

    def minus_click(e):
        txt_number.value = str(int(txt_number.value) - 1)
        page.update()

    def plus_click(e):
        txt_number.value = str(int(txt_number.value) + 1)
        page.update()

    page.add(
        ft.Row(
            [
                ft.IconButton(ft.icons.REMOVE, on_click=minus_click),
                txt_number,
                ft.IconButton(ft.icons.ADD, on_click=plus_click),
            ],
            alignment=ft.MainAxisAlignment.CENTER,
        )
    )

ft.app(main)
```

### インストール

```bash
pip install flet
```

### スクリプト実行

```bash
flet run counter.py
# => アプリはネイティブのOSのウィンドウで動く

flet run --web counter.py
# => ブラウザが新しいウィンドウまたはタブが開く
```

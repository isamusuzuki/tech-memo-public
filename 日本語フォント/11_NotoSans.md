# Noto Sans Japanese フォント

作成日 2025/01/21

## 入手方法

1. [Google Fonts](https://fonts.google.com/)に行き、「Noto Sans Japanese」を検索する
1. Noto Sans Japanese を選択して、次のページに行き、「Get font」ボタンをクリックする
1. `Noto_Sans_JP.zip`ファイルがダウンロードされる。ZIP ファイルをすべて展開する

- `NotoSansJP-VariableFont_wght.ttf` ... 9MB
- `static/NotoSansJP-xxx.ttf` ... 5MB

9 種類あるフォントウェイトの内訳

```text
Thin 100, ExtraLight 200, Light 300
Regular 400, Medium 500, Semibold 600
Bold 700, ExtraBold 800, Black 900
```

## 使い方

使いたい Java プロジェクトの resource フォルダに置く

```java
Path path = Path.of(Main.class.getClassLoader().getResource("NotoSansJP-Regular.ttf").toURI());
logger.info(path.toString());
```

# JSXコンポーネントについて

作成日 2025/10/16

## 1. 解説記事を読む

[Reactの基本!コンポーネントについて](https://qiita.com/Hashimoto-Noriaki/items/95d9fe027d169ce74218)

- ReactではJavaScriptの中でHTMLを書くことができ、ボタンやヘッダー、フッターなど、画面の部品単位でパーツを作れます。これをコンポーネントと言います。
- JavaScriptの中でHTMLで書く方法を「jsx」と言います。
- コンポーネントはJavaScriptの関数として定義します。その時関数名の先頭は大文字にします。またアロー関数でも定義できます。
- JSXが複数行の時は()で括るやり方があります。
- クラス名はclassNameにします。これはJSXの書き方です。

practice.tsx

```javascript
export const Practice = () => (
    <div className="component">
      </h3>コンポーネント</h3>
    </div>
);
```

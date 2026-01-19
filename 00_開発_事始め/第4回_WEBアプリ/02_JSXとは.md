# JSXとは

作成日 2026/01/19

## 1. JSXを説明する

JSX = JavaScript XML

JavaScriptの構文を拡張し、JavaScriptコードの中に、HTMLタグを直接書けるようにする記法

主にReactでUIコンポーネントを記述するために使われる

Honoは、JSXをサーバーサイドのHTMLテンプレートとして採用した

## 2. JSXのお約束

- JSXを使うときは、ファイルの拡張子を変更する `.js` -> `.jsx`, `.ts` -> `.tsx`
- HTMLタグを記述するときは、必ず開始タグで始まり、その閉じタグで終わること。HTMLタグを並列で記述するとエラーになる

```javascript
// オーケー
return (
    <ul>
        <li>サンプル1</li>
        <li>サンプル2</li>
    </ul>
);

// NG
return (
    <div>サンプル1</div>
    <div>サンプル2</div>
);

// オーケー
return (
    <>
        <div>サンプル1</div>
        <div>サンプル2</div>
    </>
);
```

- HTMLタグのclass属性は、JavaScriptの予約語とかぶっているので、classNameを使う
- HTMLタグのstyle属性の値は、JavaScriptのオブジェクト形式で書く

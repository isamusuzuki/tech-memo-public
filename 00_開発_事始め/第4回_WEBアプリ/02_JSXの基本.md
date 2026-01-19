# JSXの基本

作成日 2026/01/19

## 1. JSXとは？

JSX = JavaScript XML

JavaScriptの構文を拡張し、JavaScriptコードの中に、HTMLタグを直接書けるようにする記法

主にReactでUIコンポーネントを記述するために使われる

Honoは、JSXをテンプレートとして採用した

## 2. JSXのお約束

- JSXを使うときは、ファイルの拡張子を変更する `.js` -> `.jsx`, `.ts` -> `.tsx`
- HTMLタグを記述するときは、必ず開始タグで始まり、その閉じタグで終わること。HTMLタグを並列で記述するとエラーになる
- HTMLタグのclass属性は、JavaScriptの予約語とかぶっているので、classNameを使う
- HTMLタグのstyle属性の値は、JavaScriptのオブジェクト形式で書く

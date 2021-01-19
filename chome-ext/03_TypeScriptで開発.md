# TypeScript で Chrome 機能拡張を開発する

作成日 2021/01/02

## 01. 関連記事を読む

[Chrome Extensions を TypeScript で開発する \| ひよこまめ](https://blog.chick-p.work/chrome-extensions-typescript/)

=> Chrome の型定義は存在するようだ `npm install --save-dev @types/chrome`

[@types/chrome \- npm](https://www.npmjs.com/package/@types/chrome)

> This package contains type definitions for Chrome extension development (`http://developer.chrome.com/extensions/`).

## 02. 移植作業中にエラーに遭遇

JavaScript のコードを TypeScript にコピペしたら、`document.getElementById()` メソッドでエラーが出た

このメソッドで得られるものは、`HTMLElement | null` であるため、HTMLElement のつもりで話を進めると、`Object is possibly 'null'.` と注意される

TypeScript では、以下のように書くのが正しい

```typescript
const loader = document.getElementById('loader');
if (loader) {
  loader.style.display = 'none';
}
```

また、`HTMLElement.disabled` もそんなプロパティを知らないと注意される

正しくは、`HTMLInputElement.disabled` であった

```typescript
var element = <HTMLInputElement>document.getElementById('btnExcel');
element.disabled = true;
```

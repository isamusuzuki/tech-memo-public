# Turbolinks & Stimulus

作成日 2019/12/01

## 紹介記事を読む

[SPA じゃないプロジェクトのための控えめな JavaScript フレームワーク「Stimulus」 \- yuhei blog](https://yuheiy.hatenablog.com/entry/2019/05/02/204549)

Stimulus は、Basecamp のチームが開発した、ひかえめな JavaScript フレームワーク

この記事には、"The Origin of Stimulus"の日本語翻訳が載っている

## Turbolinks とは

Web アプリケーションのページ遷移を早くするツール

[turbolinks/turbolinks: Turbolinks makes navigating your web application faster](https://github.com/turbolinks/turbolinks)

> Get the performance benefits of a single-page application without the added complexity of a client-side JavaScript framework.\
> Use HTML to render your views on the server side and link to pages as usual. \
> When you follow a link, Turbolinks automatically fetches the page, \
> swaps in its `<body>`, and merges its `<head>`, \
> all without incurring the cost of a full page load.

## Stimulus とは

[Stimulus Handbook: The Origin of Stimulus](https://stimulusjs.org/handbook/origin)

> The three core concepts in Stimulus\
> Controllers, actions, and targets.

```html
<div data-controller="clipboard">
    PIN:
    <input data-target="clipboard.source" type="text" value="1234" readonly />
    <button data-action="clipboard#copy">Copy to Clipboard</button>
</div>
```

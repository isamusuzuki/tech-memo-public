# URLを作成する

作成日 2025/12/04

## 1. URLコンストラクターを使う

[URL: URL() constructor - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL)

- 構文1 => `new URL(url)`
- 構文2 => `new URL(url, base)`

```javascript
let baseUrl = "https://developer.mozilla.org";

let a = new URL("/", baseUrl);
// => 'https://developer.mozilla.org/'

let b = new URL(baseUrl);
// => 'https://developer.mozilla.org/'

new URL("en-US/docs", b);
// => 'https://developer.mozilla.org/en-US/docs'

let d = new URL("/en-US/docs", b);
// => 'https://developer.mozilla.org/en-US/docs'

new URL("/en-US/docs", d);
// => 'https://developer.mozilla.org/en-US/docs'
```

URLコンストラクターは、ただ2つの文字列を結合するだけでなく、必要なものがなければ追加するし、不要なものがあれば取り除いて、URLを作成する

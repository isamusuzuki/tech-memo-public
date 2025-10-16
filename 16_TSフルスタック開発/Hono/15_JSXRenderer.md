# JSX Renderer

作成日 2025/10/15、更新日 2025/10/16

ビルトインのミドルウェア

## 1. 公式サイトを読む

[JSX Renderer Middleware - Hono](https://hono.dev/docs/middleware/builtin/jsx-renderer)

src/index.tsx

```typescript
import { Hono } from 'hono';
import { jsxRenderer } from 'hono/jsx-renderer';

const app = new Hono();

app.get(
    '/page/*',
    jsxRenderer(({ children }) => {
        return (
            <html>
                <body>
                    <h1>My Page</h1>
                    <div>{children}</div>
                </body>
            </html>
        );
    })
);

app.get('/page/about', (c) => {
    return c.render(<h1>About me!</h1>);
});

export default app;
```

## 2. JSXの型定義とその設定方法

マウスをホバーすると型定義が表示される

- html => (property) JSX.IntrinsicElements.html: HtmlHTMLAttributes
- body => (property) JSX.IntrinsicElements.body: JSX.HTMLAttributes
- h1 => (property) JSX.IntrinsicElements.h1: JSX.HTMLAttributes
- div => (property) JSX.IntrinsicElements.div: JSX.HTMLAttributes

これはどこから来ているのか？

node_modules\hono\dist\types\jsx\intrinsic-elements.d.ts

```typescript
export declare namespace JSX {
    //600行
    export interface IntrinsicElements {
        body: HTMLAttributes;
        div: HTMLAttributes;
        h1: HTMLAttributes;
        html: HtmlHTMLAttributes;
    }
}
```

node_modules\hono\dist\types\jsx\index.d.ts

```typescript
export type { JSX } from './intrinsic-elements';
```

node_modules\hono\package.json

```json
{
    "name": "hono",
    "types": "dist/types/index.d.ts",
    "exports": {
        ".": {
            "types": "./dist/types/index.d.ts",
        },
        "./jsx": {
            "types": "./dist/types/jsx/index.d.ts",
        }
    }
}
```

正しい設定方法

tsconfig.json

```json
{
    "compilerOptions": {
        "jsx": "react-jsx",            // この行はそのまま
        "jsxImportSource": "hono/jsx", // この行を追加する
    },
    "include": ["src/**/*.tsx"] // 拡張子.tsxを追加する
}
```

## 3. DOCTYPEだけJSXの型定義がない

解決方法1: html関数で文字列を包む（おそらく古いやり方）

```typescript
import { html } from 'hono/html';

import { jsxRenderer } from 'hono/jsx-renderer';

export const renderer = jsxRenderer(({ children }) => {
    return html`<!DOCTYPE html>
        <html lang="ja">
            <head>
            </head>
            <body>
            </body>
        </html> `;
});
```

解決方法2: docTypeオプションを使う

```typescript
export const renderer = jsxRenderer(
    ({ children }) => {
        return (
            <html lang="ja">
                <head>
                </head>
                <body>
                </body>
            </html>
        );
    },
    { docType: '<!DOCTYPE html>' }
);
```

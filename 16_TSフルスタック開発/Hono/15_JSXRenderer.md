# JSX Renderer

作成日 2025/10/15

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

## 2. JSXの型定義を探る

マウスをホバーすると、型定義が表示される

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

## 3. 正しい設定方法

tsconfig.json

```json
{
    "compilerOptions": {
        "jsx": "react-jsx",            // このまま
        "jsxImportSource": "hono/jsx", // この行を追加する
    },
    "include": ["src/**/*.tsx"] // 拡張子.tsxを追加する
}
```

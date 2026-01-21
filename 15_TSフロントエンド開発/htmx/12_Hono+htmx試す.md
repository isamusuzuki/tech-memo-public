# Hono+htmxを試す

作成日 2025/10/16

## 1. Hono+htmxのサンプルコードを発見する

[Hono + htmx + Cloudflareでアプリケーションを実装してみる](https://zenn.dev/aoito/articles/dc111b83212c0b)

> 本記事は、@yusukebe 氏が提案する、「Hono + htmx + Cloudflareスタック」を使用したサーバレスアプリケーションを開発する流れを紹介します。
>
> このスタックは、通常API開発に利用される Hono と Cloudflare に、JavaScript無しで動的なページを構築できる htmx を加えることで、CDNエッジで動作するマルチページアプリケーション(MPA)を作成可能にします。

[Hono + htmx + Cloudflareは新しいスタック](https://zenn.dev/yusukebe/articles/e8ff26c8507799)

> JSXをサーバーサイドのテンプレートとして使う技術スタックを紹介します。これはつまりReactなしでJSXを使うということです。

サンプルコードのリポジトリ => [yusukebe/hono-htmx: Hono+htmx stack](https://github.com/yusukebe/hono-htmx)

## 2. サンプルコードのファイル＆ディレクトリ構造

```text
--hono-htmx/
    |--src/
    |   |--components.tsx ... ファンクション3つ renderer(),AddTodo(),Item()
    |   `--index.tsx ... Honoサーバーの定義3つ GET "/", POST "/todo", DELETE "/todo/:id"
    `--todo.sql ... D1データベース用のテーブル定義
```

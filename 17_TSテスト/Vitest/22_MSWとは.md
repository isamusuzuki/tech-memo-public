# MSWとは

作成日 2025/11/11、更新日 2025/11/12

## 1. 解説記事を読む

[フロントエンドのテストのモックには msw を使うのが最近の流行りらしい](https://zenn.dev/azukiazusa/articles/using-msw-to-mock-frontend-tests)

- フロントエンドのテストを書くときには API コールする処理を全てモックする必要がある
- 最近のテスト手法として API コールをモックする際に Jest ではなく Mock Service Worker (以下 msw ）を使用する手法が注目されている
- mswは、ローカルホストでモック用のサーバーを起動するのではなく、サービスワーカーレベルでリクエストをインターセプトしてリクエストを返却する
- msw を使用すると、ネットワークレベルでモック化されるので axios の実装までもテスト対象に含まれせることができる

## 2. 公式サイト（英語）を読む

[Mock Service Worker - API mocking library for browser and Node.js](https://mswjs.io/)

> Mock Service Worker is an API mocking library that allows you to write client-agnostic（※1） mocks and reuse them across any frameworks, tools, and environments.

※1 agnostic = 神の存在などに確信できないと考える人。「clientからは、モックの存在を知りようもない」という意味

[Quick start - Mock Service Worker](https://mswjs.io/docs/quick-start)

インストール => `npm i msw --save-dev`

## 3. サンプルコードを覗く

[mswjs/examples: Examples of Mock Service Worker usage with various frameworks and libraries.](https://github.com/mswjs/examples)

## 4. v2で大きな変更がある

[MSW（Mock Service Worker）をv2.0.0にアップグレードする](https://zenn.dev/keitakn/scraps/2ca70305a71847)

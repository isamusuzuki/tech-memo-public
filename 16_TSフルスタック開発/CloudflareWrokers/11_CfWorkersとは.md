# Cloudflare Workersとは

作成日 2025/09/10、更新日 2025/10/10

## 1. 公式サイトを読む

[Cloudflare Workers | サーバーレスアプリケーションを構築 | Cloudflare](https://www.cloudflare.com/ja-jp/developer-platform/products/workers/)

> Cloudflareのグローバルネットワークの全世界330か所以上のデータセンターへのグローバルにデプロイ時に\
> インフラの設定や保守は無用でサーバーレス機能やアプリケーションを構築。
>
>- Cloudflare WorkersはコンテナではなくV8分離で稼働
>- 起動時にインスタンスへのランタイム読み込みを待つ遅延が一切ない
>- 瞬時のスタートアップタイムが実現
>- 必要なのは呼び出し時に読み込まれるコードのみ
>- 実質的にリクエスト時のコールドスタートが存在しない

## 2. プロジェクトを開始する

以下を参照すべし

- [D1を試す](./13_D1を試す.md)
- [DrizzleORMを試す](../DrizzleORM/13_DrizzleORMを試す.md)
- [Zodを試すa](../Zod/12_Zodを試すa.md) + [Zodを試すb](../Zod/13_Zodを試すb.md)
- [vite-create-template](./12_vite-react-template.md)

## 3. つまづいた所

cloudflare-workersの管理ツールであるwranglerは、開発サーバーも兼ねているが、\
開発コンテナの中で開発サーバーを利用するためには、Viteの時と同じように、\
外部PCからの開発サーバーへのアクセスを許可する必要がある

```json
{
    "dev": {
        "ip": "0.0.0.0"
    }
}
```

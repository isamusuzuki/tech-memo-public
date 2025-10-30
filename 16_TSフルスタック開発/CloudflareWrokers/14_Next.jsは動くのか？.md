# Next.jsをCloudflareで動かすことはできるのか？

作成日 2025/10/31

## 1. 解説記事を読む

[遂に Cloudflare + Next.js(OpenNext) + Prisma 6.7.0(No Rust) が動く時代が来た](https://zenn.dev/mizchi/articles/cloudflare-opennext-prisma-no-rust)

> opennext for cloudflare でできるようになった。open といいつつ、ほぼ Cloudflare で Next.js を動かすプロジェクトと理解して大丈夫です。実質的に next-on-pages の後継です。

## 2. 公式サイト（英語）を読む

[OpenNext > Cloudflare](https://opennext.js.org/cloudflare)

```bash
npm create cloudflare@latest -- my-next-app --framework=next --platform=workers
```

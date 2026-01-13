# package.jsonのexports項目

作成日 2025/10/22

## 1. 解説記事を読む

[package.jsonのexportsフィールドについて](https://zenn.dev/makotot/articles/5edb504ef7d2e6)

> npm パッケージとして複数のエントリーポイントを公開したい場合、`main`フィールドは単一のエントリーポイントしか受け付けないので、`exports`フィールドを利用することになる

## 2. 公式ドキュメント（英語）を読む

[Node.js v25.0.0 documentation > Modules: Packages > Package entry points](https://nodejs.org/api/packages.html#package-entry-points)

パッケージの`package.json`ファイルでは、そのパッケージのエントリーポイントを定義できるフィールドが2つある。すなわち`main`と`exports`である。どちらのフィールドも、ESモジュールとCommonJSモジュール両方のエントリーポイントに適用される。

`main`フィールドは、Node.jsの全てのバージョンでサポートされているが、できることが限られていて、そのパッケージのメインのエントリーポイントしか定義できない。

`exports`フィールドは、`main`の新しい代替で、複数のエントリーポイントを定義できる。

新しいパッケージを開発するときは、両方を定義しておくことをお勧めする

```json
{
    "main": "./index.js",
    "exports": "./index.js"
}
```

ターゲットは相対URLで書き、`./`で始めること

```json
{
    "exports": {
        ".": "./dist/main.js",
        "./feature": "./lib/feature.js"
    }
}
```

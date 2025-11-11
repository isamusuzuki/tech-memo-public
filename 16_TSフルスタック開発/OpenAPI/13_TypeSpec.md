# TypeSpec

作成日 2025/04/25、更新日 2025/11/11

## 1. 解説記事aを読む

[4万行超のopenapi.yamlをTypeSpecに移行した話](https://zenn.dev/yuta_takahashi/articles/migrate-to-typespec)

> TypeSpec はエミッターと呼ばれる標準ライブラリを備えています。エミッターを使うことで、\
> TypeSpec を SSoT として OpenAPI3, JSON Schema, Protobuf 等の様々なスキーマを自動生成することが可能です。
>
> さらに、2025 年 4 月現在では、JavaScript, Java, Python, CSharp 向けのクライアントエミッターもプレビューとして公開されています。\
> 近い将来、TypeSpec から直接クライアントコードを生成することがスタンダードになるかもしれません。

- エミッター => emitするヒト => emit: 発する => 発するヒト
- SSoT => Single Source of Truth => 信頼できる唯一の情報源

> もし 何らかの理由で TypeSpec から撤退することになっても、\
> 自動生成された openapi.yaml を直接利用する方針へ容易に転換することができます。
>
> この撤退のしやすさにより、短期間で移行に踏み切る意思決定ができました。

## 2. 解説記事bを読む

[YAMLで病むる人への処方箋【TypeSpec】](https://zenn.dev/kitamago/articles/b71399388965c0)

> 私の常駐先では、API開発はOpenAPIのSpecファイル(yaml)を作成し、これに基づきOrvalで生成された型情報を用いてフロントエンドとバックエンドを同時に開発しています。
>
> TypeSpecとは、TypeScriptライクな書き心地で Open API Spec がかけるDSL（安心安全のマイクロソフト謹製）で、プログラミング言語のような書き心地でコード分割・再利用ができる。

使てみてよかった点

- VSCodeの拡張機能を入れれば、自動補完が効くし、フォーマッター・リンターがデフォルトで存在する（ありがたい）。シンプルに開発体験が良い。
- `@visibility`を使うことでCRUD別のプロパティの切り替えを簡単に実現できる

[TypeSpec - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=typespec.typespec-vscode)

## 3. 公式サイト（英語）を読む

[typespec.io](https://typespec.io/)

> Design your data up front and generate schemas, API specifications, client / server code, docs, and more.
>
> With TypeSpec, remove the handwritten files that slow you down, and generate standards-compliant API schemas in seconds.

main.tsp

```javascript
import "@typespec/http";

using Http;

model Store {
  name: string;
  address: Address;
}

model Address {
  street: string;
  city: string;
}

@route("/stores")
interface Stores {
  list(@query filter: string): Store[];
  read(@path id: Store): Store;
}
```

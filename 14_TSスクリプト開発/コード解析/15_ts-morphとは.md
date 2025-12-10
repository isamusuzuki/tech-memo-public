# ts-morphとは

作成日 2025/11/25

## 1. 解説記事aを読む

[600ファイル5000箇所の多言語対応を半日で終わらせた話](https://zenn.dev/dress_code/articles/codebase-transformation)

> ts-morph は、TypeScript の AST を操作するためのライブラリです。
> TypeScript Compiler API をラップしており、型安全にコードの解析・変換ができます。
> 正規表現では困難な「構造を理解した上での変更」が可能で、大規模なコードベース変換には最適なツールです。

```javascript
import { Project, SyntaxKind } from "ts-morph";

const project = new Project({
  tsConfigFilePath: "./tsconfig.json",
});

// 対象ファイルを再帰的に探索
const sourceFiles = project.getSourceFiles("src/**/*.ts");

sourceFiles.forEach((sourceFile) => {
  // DictSetObjectパターンの検出と処理
  const objectLiterals = sourceFile.getDescendantsOfKind(
    SyntaxKind.ObjectLiteralExpression
  );

  objectLiterals.forEach((obj) => {
    const properties = obj.getProperties();
    const hasLangKeys = ["ja", "en", "id", "vi", "th"].every((lang) =>
      properties.some((p) => p.getName() === lang)
    );

    if (hasLangKeys && !properties.some((p) => p.getName() === "zhCN")) {
      // 翻訳マーカーを挿入
      obj.addPropertyAssignment({
        name: "zhCN",
        initializer: "'__TRANSLATE__'",
      });
    }
  });

  sourceFile.saveSync();
});
```

=> ts-morphは、TypeScriptの構文解析をするだけではなくて、ASTで改修箇所を検出して、そのまま改修するツールなのだと理解した

## 2. 解説記事bを読む

[TypeScriptのコード分析を楽にする ts-morph入門](https://zenn.dev/gmomedia/articles/127ca64e9b7018)

> ts-morphは、公式のコンパイラAPIをラップして、より使いやすいインターフェースを提供します。
>
> 以下のような作業を安全に自動化できます：
>
>- コードの解析：クラスやインターフェース、関数の定義を探して、その内容を調べる
>- 型情報の取得：変数や関数の型を正確に把握する
>- コードの修正：メソッド名の変更や、新しいプロパティの追加を安全に行う
>- コードの生成：型定義を元に、新しいコードを自動生成する

### 基本的な解析例

サンプルのTypeScriptコード

```javascript
interface User {
  id: number;
  name: string;
  age?: number;
}

class UserService {
  private users: User[] = [];

  findById(id: number): User | undefined {
    return this.users.find(user => user.id === id);
  }
}
```

ts-morphでコード解析

```javascript
import { Project } from "ts-morph";

const project = new Project({
  tsConfigFilePath: "./tsconfig.json"
});

const sourceFile = project.addSourceFileAtPath("src/sample.ts");

// インターフェースの解析
const userInterface = sourceFile.getInterfaceOrThrow("User");
const properties = userInterface.getProperties();

// プロパティの情報を取得
properties.forEach(prop => {
  console.log({
    name: prop.getName(), // プロパティ名
    type: prop.getType().getText(), // プロパティの型
    questionToken: prop.hasQuestionToken() // オプショナルかどうか
  });
});

// メソッドの解析
const userService = sourceFile.getClassOrThrow("UserService");
const findById = userService.getMethodOrThrow("findById");
```

ts-morphでコード修正

```javascript
// findByIdメソッドをgetByIdに変更
const method = userService.getMethodOrThrow("findById");
method.rename("getById");

// 新しいプロパティの追加
userInterface.addProperty({
  name: "email",
  type: "string",
  hasQuestionToken: true  // オプショナルにする
});
```

### ts-morphの便利な使い方

ts-morphの多くのメソッドには2つのバージョンがある

- `getClass()` ... 失敗時にundefinedを返す（存在が不確実な場合）
- `getClassOrThrow()` ... 失敗時に例外をスロー（存在が確実な場合）

ts-morphでの変更は以下の手順で行う

1. 必要な変更をメモリ上で実行
2. 全ての変更が成功したことを確認
3. `project.save()`で一括保存

## 3. npmサイト（英語）を読む

[ts-morph - npm](https://www.npmjs.com/package/ts-morph)

> TypeScript Compiler API wrapper. Provides an easier way to programmatically navigate and manipulate TypeScript and JavaScript code.

インストール => `npm i ts-morph`

GitHub => [dsherret/ts-morph: TypeScript Compiler API wrapper for static analysis and programmatic code changes.](https://github.com/dsherret/ts-morph)

公式サイト => [ts-morph - Documentation](https://ts-morph.com/)

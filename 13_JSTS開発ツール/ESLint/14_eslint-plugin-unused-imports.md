# eslint-plugin-unused-imports.md

作成日 2025/09/24

## 公式サイト（英語）を読む

[sweepline/eslint-plugin-unused-imports: Package to separate no-unused-vars and no-unused-imports for eslint as well as providing an autofixer for the latter.](https://github.com/sweepline/eslint-plugin-unused-imports)

不要なES6モジュールのインポートを見つけて削除します。この機能は、AST（抽象構文木）内でインポート文であるかどうかに応じて`no-unused-vars`ルールを分割し、インポートであるノードを自動修正で削除するルールを提供することで実現します。このプラグインは、TypeScriptまたはJavaScriptプラグインの`no-unused-vars`ルールを組み合わせて動作するため、これらのプラグインが正しくインストールされ、レポートを出力している必要があります。

### インストール（ESLintの導入は終わっているものとする）

```bash
npm install eslint-plugin-unused-imports --save-dev
```

### 設定（すでにあるeslint.config.jsファイルに追記する）

```javascript
import unusedImports from "eslint-plugin-unused-imports";

export default [{
    plugins: {
        "unused-imports": unusedImports,
    },
    rules: {
        "no-unused-vars": "off", // or "@typescript-eslint/no-unused-vars": "off",
        "unused-imports/no-unused-imports": "error",
        "unused-imports/no-unused-vars": [
            "warn",
            {
                "vars": "all",
                "varsIgnorePattern": "^_",
                "args": "after-used",
                "argsIgnorePattern": "^_",
            },
        ]
    }
}];
```

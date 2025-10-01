# ESLint 機能拡張

作成日 2025/01/10

[Marketplace](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

- オーナー: Microsoft
- 識別子: dbaeumer.vscode-eslint

## 設定ファイル

公式マニュアル（英語） => [Configuration Files](https://eslint.org/docs/latest/use/configure/configuration-files)

```javascript
// eslint.config.js
export default [
    {
        rules: {
            semi: "error",
            "prefer-const": "error"
        }
    }
];

// type: module in package.json
module.exports = [
    {
        rules: {
            semi: "error",
            "prefer-const": "error"
        }
    }
];
```

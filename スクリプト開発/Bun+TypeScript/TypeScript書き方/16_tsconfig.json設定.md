# tsconfig.json設定

作成日 2025/07/08

## 1. compilorOptions > typeRoots

型定義ファイル(d.ts)を置くフォルダを指定する

```json
{
    "compilorOptions" {
        "typeRoots": [
            "node_modules/@types",
            "src/@types"
        ]
    }
}
```

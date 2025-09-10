# tsconfig.json応用

作成日 2025/09/10

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

## 2. rootDirとbaseUrl

[tsconfig.jsonのrootDirとbaseUrlに関するメモ](https://qiita.com/Nekonecode/items/09b26deec21a5f83adb1)

### 3a. rootDir

- TypeScriptのソースコードがあるディレクトリを指定する
- 指定したディレクトリの外にあるソースコードは参照できなくなる

### 3b. baseUrl

- 絶対パス指定じゃない場合、どこを基準のフォルダにするか？というパラメータ

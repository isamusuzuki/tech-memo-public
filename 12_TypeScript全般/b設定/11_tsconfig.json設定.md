# tsconfig.json設定

作成日 2025/09/09、更新日 2025/09/17

## 1. 解説記事を読む

[なんとなく使うtsconfig.jsonをやめる](https://zenn.dev/uniformnext/articles/e2106ba4d995b1)

ルート項目

- `include` ... コンパイル対象とするファイルやフォルダを指定
- `exclude` ... コンパイルから除外するファイルやフォルダを指定

compilerOptions項目

- `target` ... `ESNext`,`ES2020`,`ES5`
- `module` ... `ESNext`,`CommonsJS`,`ES6`
- `outDir` ... コンパイル後のファイルの出力先
- `rootDir`
- `strict`

## 2. typeRoots

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

## 3. rootDirとbaseUrl

[tsconfig.jsonのrootDirとbaseUrlに関するメモ](https://qiita.com/Nekonecode/items/09b26deec21a5f83adb1)

### rootDir

- TypeScriptのソースコードがあるディレクトリを指定する
- 指定したディレクトリの外にあるソースコードは参照できなくなる

### baseUrl

- 絶対パス指定じゃない場合、どこを基準のフォルダにするか？というパラメータ

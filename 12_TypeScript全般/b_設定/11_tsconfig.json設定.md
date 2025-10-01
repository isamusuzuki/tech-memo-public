# tsconfig.json設定

作成日 2025/09/09、更新日 2025/10/01

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

## 2. compilerOptions > typeRoots

型定義ファイル(*.d.ts)を置くフォルダを指定する

tsconfig.jsonからの相対パスを書く

```json
{
    "compilorOptions" {
        "typeRoots": [
            "./@types"
            "./node_modules/@types",
        ]
    }
}
```

TypeScriptファイルでは、相対パスを書く必要がなくなる

```javascript
// @types/instruction.d.tsファイルを指定するとき
import type { Instruction } from 'instruction';
```

## 3. compilerOptions > paths

[tsconfig.json の paths](https://zenn.dev/hayato94087/articles/9f3bf702543431)

> paths を利用することでエイリアスを利用しファイルへアクセスできます。

```json
{
    "compilorOptions" {
        "paths": {
            "@/*": ["./*"]
        }
    }
}
```

- TypeScriptファイルでは、相対パスを書く必要がなくなる
- 細かくフォルダを羅列せず、`@/*`1個ですべてをまかなう
- `@logtape/logtape`のようなnpmパッケージとの違いを出す

```javascript
import type { Logger } from '@logtape/logtape';
import { readFileInGakumu } from '@/utils/readFile.ts';
import { writeTableText } from '@/ast/writeTableText.ts';
```

## 4. rootDirとbaseUrl

[tsconfig.jsonのrootDirとbaseUrlに関するメモ](https://qiita.com/Nekonecode/items/09b26deec21a5f83adb1)

rootDir

- TypeScriptのソースコードがあるディレクトリを指定する
- 指定したディレクトリの外にあるソースコードは参照できなくなる

baseUrl

- 絶対パス指定じゃない場合、どこを基準のフォルダにするか？というパラメータ

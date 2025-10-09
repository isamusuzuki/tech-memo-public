# tsconfig.json設定

作成日 2025/09/09、更新日 2025/10/09

## 1. includeとrootDirの違い

解説記事1 => [なんとなく使うtsconfig.jsonをやめる](https://zenn.dev/uniformnext/articles/e2106ba4d995b1)

解説記事2 => [[TypeScript] error TS6059: File is not under 'rootDir'. の対処法](https://qiita.com/masato_makino/items/bf640a253d56b708fe0b)

以下のように指定することで、コンパイルしたときに、`src -> dist`とフォルダ構造が保たれるようになる

```json
{
    "compilerOptions": {
        "rootDir": "./src",
        "outDir": "./dist",
    }
}
```

しかし、rootDirの外に`.ts`ファイルがあると、tscがエラーを出す

以下のように指定すると、rootDirの外にある`.ts`ファイルを無視するようになる

```json
{
    "include": ["src"]
}
```

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

> paths を利用することでエイリアスを利用してファイルへアクセスできます。

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

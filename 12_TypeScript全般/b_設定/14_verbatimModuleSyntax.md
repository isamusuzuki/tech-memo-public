# verbatimModuleSyntax

作成日 2025/10/21

## 解説記事を読む

[TypeScript 5.0 で追加された verbatimModuleSyntax とは何か？](https://zenn.dev/teppeis/articles/2023-04-typescript-5_0-verbatim-module-syntax)

>- 現在、TypeScript では`import type`構文やインラインの type 修飾子を使う`import {type Foo}`構文が使えるようになり、トランスパイル時に削除すべき import 文を明示的に宣言可能になっている。しかしながら、type をつけずに従来通りに型を import することもできてしまう。
>- `verbatimModuleSyntax`オプションを有効にした場合、import 文を逐語的に（verbatim, 一語一語をそのまま）トランスパイルする。import type 文は丸ごと削除され、インライン type 修飾子はその識別子のみ削除される。
>- verbatimModuleSyntaxが有効なときは型のみを import する場合に import type 構文またはインライン type 修飾子がついてないとエラーになる。
>- ただし、verbatimModuleSyntaxを有効化すると、import 文は import 文にしかトランスパイルできない。

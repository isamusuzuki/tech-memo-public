# REST と RESTful

作成日 2019/11/30

## 01. REST とは

REST = Representational State Transfer

Web アプリにおける設計概念のひとつ

すべてのリソースに一意の識別子(URI)をつけ、\
そのリソースに対して動作を決めてアクセスする

-   `/products/1`という URI に対して GET することで、1 番の商品を表示する
-   `/products/1`という URI に対して PUT することで、1 番の商品を更新する
-   `/products/1`という URI に対して DELETE することで、1 番の商品を削除する
-   `/products`という URI 対して POST することで、新しい商品を保存する

REST の概念にのっとったシステムは`RESTful`といわれる

## 02. RESTful とは

分散型システムにおける複数のソフトウェアを連携させるのに適した\
設計原則の集合、考え方のこと。Roy Fielding が 2000 年に提唱した

### URI の命名規則

URI 名は「リソース本位」にするのが RESTful だと言える。\
なぜならば、動詞はメソッド名の中にもうあるのだから

-   GET ... select, search, list
-   POST ... create, assign, allocate
-   PUT ... update
-   DELETE ... remove, release

#### 【おまけ】assign と allocate の違い

-   I didn't allocate enough time to finish my assignment.
-   As punishment, he was assigned to cleaning toilets for the rest of the week.

「割り当てる」という意味では、どちらも正しいと言える。\
興味深いことは、どちらも A to B の形になっていること。

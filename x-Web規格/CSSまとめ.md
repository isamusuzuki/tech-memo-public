# CSSまとめ

作成日 2018/02/18、最終更新日 2018/04/27

## vhという単位の意味

- vw ... ビューポートの横幅の100分の1
- vh ... ビューポートの高さの100分の1

[\[CSS\]ビューポート\(vw, vh\)とパーセント\(%\)、レスポンシブに適した単位の賢い使い分け方法 \| コリス](https://coliss.com/articles/build-websites/operation/css/viewport-vs-percentage-units-by-ire.html)

## CSS Flexible Box Layoutとは

CSSで`display: flex`と指定すること。子クラスを横並びにできる

Flexboxは、すべてのブラウザでサポートされている => [Can I use CSS Flexible Box Layout Module](https://caniuse.com/#feat=flexbox)

### [これからのCSSレイアウトはFlexboxで決まり！](https://www.webcreatorbox.com/tech/flexbox)を写経する

Flexboxのメリット

- 親クラスにCSSを1行プラスするだけで子クラスが横並びになる
- 横並びになった要素の高さが最初から揃っている
- 要素を上下左右、好きな順序に並べられる
- スペースの操作も自由自在
- 高さの異なる要素を横に並べても、上下中央が簡単に揃う

```css
.main-nav {
  /* 子クラスを横並びにする */
  display: flex;
  /* 最初の要素を左端に、最後の要素を右端に置き、残りの要素を等間隔に並べる */
  justify-content: space-between;
  /* 要素を右端に並べる */
  justify-content: flex-end;
  /* 要素を中央揃えに並べる */
  justify-content: center;
}
```

子クラスの横幅を調整する

```css
.main {
  /* 子クラスを横並びにする */
  display: flex;
}

.main section {
  /* 以下の3つのショートハンド */
  flex: 0 1 auto;
  /* flexアイテムの伸びる倍率を設定 */
  flex-grow: 0;
  /* flexアイテムの縮む倍率を設定 */
  flex-shrink: 1;
  /* flexアイテムの横幅を指定する */
  flex-basis: auto;
}
```

子クラスの並びを調整する

```css
.main {
  /* 子クラスを横並びにする */
  display: flex;
  /* 高さの異なる子クラスを垂直に中央揃えする */
  align-items: center;
  /* 子クラスをベースラインに合わせる */
  align-items: baseline;
  /* 高さの異なる子クラスを上に揃える */
  align-items: flex-start;
  /* 高さの異なる子クラスを下に揃える */
  align-items: flex-end;
  /* flexアイテムが伸ばされて揃えられる（初期値）*/
  align-items: stretch;
}
```

レスポンシブに対応する

```css
.main {
  /* 子クラスを横並びにする */
  display: flex;
}
@media screen and (max-width: 700px) {
  .main {
    flex-direction: column;
  }
}
```

### [「flexアイテム」の幅の計算方法](https://parashuto.com/rriver/development/how-flex-item-width-is-calculated)を読む

- `display:flex`を指定した親要素 => flexコンテナ
- その子要素 => flexアイテム

スペースが余っている場合、flexアイテムは、`flex-grow`の指示に従って余ったスペースを分配する

flexプロパティは、3つのショートハンド。flexプロパティの初期値は`0 1 auto`だが、
flexに1つだけ数値を指定したら、`number 1 0`となる

```css
.main section {
  /* flexに1つだけ数値を指定する */
  flex: 1;
  /* 上記と同じ意味 */
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 0;
}
```

## CSS Grid Layoutとは

ほぼ全てのブラウザが対応している => [Can I use CSS Grid Layout](https://caniuse.com/#feat=css-grid)

「[CSS Grid Layout を極める！（基礎編）](https://qiita.com/kura07/items/e633b35e33e43240d363)」を写経する

- コンテナ ... グリッド全体を囲む要素、`display: grid;`を指定する
- アイテム ... コンテナの直接の子要素
- `grid-template-rows` ... 行の高さを区切って指定
- `grid-template-columns` ... 列の高さを区切って指定
- `1fr` ... 残り全部の意味
- `grid-row` ...アイテムが占める行のラインの番号を区切って指定
- `grid-column` ... アイテムが占める列のラインの番号を区切って指定

```html
<div id="container">
    <div id="itemA">A</div>
    <div id="itemB">B</div>
    <div id="itemC">C</div>
</div>
<style>
#container {
    display: grid;
    grid-template-rows: 100px 50px;
    grid-template-columns: 150px 1fr;
}
#itemA {
    grid-row: 1 / 3;
    grid-column: 1 / 2;
    background: #f88;
}
#itemB {
    grid-row: 1 / 2;
    grid-column: 2 / 3;
    background: #8f8;
}
#itemC {
    grid-row: 2 / 3;
    grid-column: 2 / 3;
    background: #88f;
}
</style>
```

flexboxとの違い

- `display:grid` ... 配置の次元が2次元（縦横自由に配置）
- `display:flex` ... 配置の次元が1次元（折り返しするけど一方向）

## グーグルNotoフォントを指定する

```css
@import url(https://fonts.googleapis.com/earlyaccess/notosansjp.css);
body { font-family: 'Noto Sans JP', sans-serif; }
```

- グーグルNotoフォントを使えば、PCでもiPhoneでも同じになる
- グーグルNotoフォントは、iPhoneではシステムフォントに似ている
- iPhoneで表示できる日本語フォントは「ヒラギノ角ゴPro W3」と「ヒラギノ角ゴPro W6」の2つ
- Androidに標準インストールされているフォント「Droid Sans Japanese」にはボールドがない
- 游ゴシック体はPCとMacの両方で採用されたが、iPhoneで表示できないので、選択する理由はない

## font-familyプロパティを復習する

次の5つの総称名でフォントが分類されている。これらを`generic family`と呼ぶ

- `sans-serif` ... ゴシック体（ひげ無し）
- `serif` ... 明朝体（ひげ付き）
- `monospace` ... 等幅フォント
- `fantasy` ... 装飾体（花文字、ポップ体など）
- `cursive` ... 手書き風

```css
/* 一重引用符でくくる */
/* 総称名はくくらない */
/* 先に見つかったものが採用される */
/* 大文字、小文字の区別はない */
body { font-family: 'ヒラギノ角ゴ Pro W3', 'Hiragino Kaku Gothic Pro', 'メイリオ', 'Meiryo', sans-serif; }
```

## 背景画像をブラウザいっぱいに表示する

```css
body {
  /* 画像ファイルの指定 */
  background-image: url(images/background-photo.jpg);
  /* 画像を常に天地左右の中央に配置 */
  background-position: center center;
  /* 画像をタイル状に繰り返し表示しない */
  background-repeat: no-repeat;
  /* コンテンツの高さが画像の高さより大きい時、動かないように固定 */
  background-attachment: fixed;
  /* 表示するコンテナの大きさに基づいて、背景画像を調整 */
  background-size: cover;
  /* 背景画像が読み込まれる前に表示される背景のカラー */
  background-color: #464646;
}
/* バックグランド画像をいっぱいに伸ばす、レスポンシブ対応 */
header { background-size: 100%; }
```

## `display:table`で段組を実現する

```html
<div class="container">
  先行するコンテンツ
  <div class="blocks">
    <div class="block">テキスト</div>
    <div class="block">テキスト</div>
    <div class="block">テキスト</div>
  </div>
  後続するコンテンツ
</div>

<style>
  .container { width: 450px; margin: 0 auto; }
  .blocks { display: table; margin: 20px 0; }
  .block { display: table-cell; width: 130px; padding: 9px; border: 1px solid #666; }
</style>
```

- `display:table`を使えば、すべてのカラムの高さが揃う
- `disolay:table-cell`でレイアウトされたカラムは vertial-alignが有効
- floatプロパティを使うよりも、崩れにくく柔軟

## テーブルの見た目を綺麗にする

```css
/* テーブルに横縞を入れる */
tr:nth-child(even) { background: #ccc }
tr:nth-child(odd) { background: #fff }

/* リストに繰り返しアクセントをつける */
/* 3,8,13,18,23行目で太文字にする  */
li:nth-child(5n+3) { font-weight: bold; }

/* テーブルに縦縞を入れる */
/* 最初の列は黄色くして、3,5,7行目で背景を入れる */
col:first-child { background: #ff0; }
col:nth-child(2n+3) { background: #ccc; }
```

## overflowプロパティを復習する

ボックスの範囲内に内容が入りきらない場合に、はみ出た部分の表示の仕方を指定する

overflow-xプロパティとoverflow-yプロパティに分けることで、
縦・横の指定を分けることができる

- visible ... ボックスからはみ出して表示される。これが初期値
- scroll ... 入りきらない内容はスクロールして見られる
- hidden ... はみ出た部分は表示されない
- auto ... ブラウザに依存する

## positionプロパティを復習する

- 親ボックスで`position:relative`を指定する
- 子ボックスで`position:absolute`を指定する
- 親ボックスの左上を基準にして、`left:XXpx; top:YYpx`を指定して、子ボックスの位置を決める
- 親ボックスでの指定を忘れたまま、子ボックスでabsoluteを指定すると、子ボックスはウィンドウ全体の左上が基準になる

- `position:static` ... 初期値、positionが効かない状態
- `position:relative` ... 相対位置への配置
- `position:absolute` ... 絶対位置への配置
- `position:fixed` ... スクロールしても位置が固定

## marginプロパティとpaddingプロパティを復習する

- marginプロパティ ... 領域間のスペース、マイナス指定可
- paddingプロパティ ... 領域内のスペース、マイナス指定不可

- 値を1つ指定する [上下左右]
- 値を2つ指定する [上下] [左右]
- 値を3つ指定する [上] [左右] [下]
- 値を4つ指定する [上] [右] [下] [左]

## セレクタを復習する

```css
/* 複数のセレクタ */
h1, h2 { color; blue; }

/* 子孫セレクタ */
/* Aの階層下のすべてのBに適用 */
p strong { color: blue; }

/* 子セレクタ */
/* Aの1階層下のBに適用 */
p > strong { color: blue; }

/* 隣接セレクタ */
/* Aの直後のBに適用 */
h1 + p { color: blue; }

/* 擬似要素 */
/* 要素の直前と直後に適用される */
blockquote::before { content: "【"; }
blockquote::after { content: "】"; }
```

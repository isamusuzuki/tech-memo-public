# pas(Pascal)文法

作成日 2025/06/17

- `*.dpr` ... programのある主ファイル(Delphi PRoject file)
- `*.pas` ... unitのある副ファイル(PAScal file)

## 1. 主ファイルの構造

参照記事 => [Pascal の文法について --- 簡単なまとめ](https://jun-makino.sakura.ne.jp/kougi/jousho/pascal_memo.html)

- `program name (inputFile, outputFile); begin execute1; ...; executeN; end.` ... プログラム構造
- `procedure name (param1; ...; paramN);` ... 手続き頭部
- `function name (param1; ...; paramN);` ... 関数頭部

## 2. 副ファイルの構造

参照記事 => [標準 Pascal 範囲内での Delphi 入門](https://qiita.com/ht_deko/items/afb3c008a83a20792169)

```pascal
unit name;

interface
  { 宣言部: クライアントが使用できるもの }
  uses name1 ... nameN;
  { 定数,型,変数,手続き,関数の宣言 }

implementation
  { 実装部 ユニット内部で使用されるもの }
  uses name1 ... nameN;
  { 手続き,関数の実装 }

initialization
  { 初期化部 }

finalization
  { 終了処理部 }

end.
```

Pascalでは、コメントは波括弧を使うらしい

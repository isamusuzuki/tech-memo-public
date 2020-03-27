# MathJax を使って数式を作成する

作成日 2019/12/17、更新日 2020/02/25

## 01. MathJax とは

JavaScript で書かれた数式表示エンジン

公式トップ => [https://www.mathjax.org/](https://www.mathjax.org/)

ドキュメント => [MathJax Documentation — MathJax 3\.0 documentation](https://docs.mathjax.org/en/latest/index.html)

## 02. MathJax が使えるところ

[https://qiita.com/](Qiita) は MathJax をサポートしている。Code 文の言語指定を math にする

```text
e^{i\pi} = -1
```

[HackMD](https://hackmd.io) も MathJax をサポートしている。

### 数式を作成して、どこでも使えるように画像にする

1. [HackMD](https://hackmd.io)に行き、Google アカウントでログインする
1. 新しいノートを作成し、MathJax を書く
1. プレビューで表示されている数式をスクリーンショットで撮る
1. PNG 画像として保存する

## 03. MathJax の使い方

[How to use MathJax & UML \- HackMD](https://hackmd.io/c/tutorials/%2Fs%2FMathJax-and-UML)

[Easy Copy MathJax](https://easy-copy-mathjax.xxxx7.com/)

### 文章の中に数式を埋め込む

ダラーで囲み、さらにその外側を空白で囲む

- 上付き、指数 ... $x^2$
- 下付き ... $x_2$
- 分数 ... $\frac{1}{x}$
- 平方根 ... $\sqrt{x}$
- 円周率 ... $\pi$
- ベクトル ... $\vec{a} = (a_1, a_2, \cdots, a_n)$
- 角度 ... $180^\circ$
- 対数 ... $log_e(x)$

### 単独で数式を表示する

ダラー 2 個の行で挟む

```text
$$
e^{i\theta}=\cos\theta+i\sin\theta
$$

$$
Precision=\frac{TP}{TP+FP}
$$

$$
Recall=\frac{TP}{TP+FN}
$$
```

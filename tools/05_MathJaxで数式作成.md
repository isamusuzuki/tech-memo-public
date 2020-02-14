# MathJax を使って数式を作成する

作成日 2019/12/17

## 01. MathJax とは

JavaScript で書かれた数式表示エンジン

公式トップ => [https://www.mathjax.org/](https://www.mathjax.org/)

ドキュメント => [MathJax Documentation — MathJax 3\.0 documentation](https://docs.mathjax.org/en/latest/index.html)

Qiita でも、MathJax はサポートされている。Code 文の言語指定を math にするだけ

```text
e^{i\pi} = -1
```

## 02. MathJax の使い方

[How to use MathJax & UML \- HackMD](https://hackmd.io/c/tutorials/%2Fs%2FMathJax-and-UML)

### 数式を文章の中に埋め込みたい

ダラーで囲み、さらに空白で囲む

-   上付き、指数 ... $x^2$
-   下付き ... $x_2$
-   分数 ... $\frac{1}{x}$
-   平方根 ... $\sqrt{x}$
-   円周率 ... $\pi$
-   ベクトル ... $\vec{a} = (a_1, a_2, \cdots, a_n)$
-   角度 ... $180^\circ$
-   対数 ... $log_e(x)$

### 数式を単独で表示したい

ダラー 2 個の行で挟む

$$
e^{i\theta}=\cos\theta+i\sin\theta
$$

$$
Precision=\frac{TP}{TP+FP}
$$

$$
Recall=\frac{TP}{TP+FN}
$$

## 03. MathJax のチートシート

[Easy Copy MathJax](https://easy-copy-mathjax.xxxx7.com/)

# Katex で数式作成

作成日 2020/08/28

## 01. Katex とは

MathJax より変換が早い数式レンダラー

[KaTeX – The fastest math typesetting library for the web](https://katex.org/)

### Markdown Preview Enhanced の数式サポート

[Math Typesetting](https://shd101wyy.github.io/markdown-preview-enhanced/#/math)

> KaTeX is faster than MathJax, but it lacks many features that MathJax has. You can check KaTeX supported functions/symbols.

[Supported Functions · KaTeX](https://katex.org/docs/supported.html)

## 02. 数式の位置合わせ

以下の書き方は、Markdown Preview Enhanced がエラーを吐く

```text
\begin{eqnarray}
x + 2x &=& 3 \\
x &=& 1
\end{eqnarray}
```

eqnarray の代わりに、aligned を使うと正しく表示できる

```text
$$
\begin{aligned}
x + 2x &= 3 \\
x &= 1
\end{aligned}
$$
```

$$
\begin{aligned}
x + 2x &= 3 \\
x &= 1
\end{aligned}
$$

KaTeX は eqnarray をサポートしていないがわかった

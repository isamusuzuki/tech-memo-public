# Matplotlib.pyplot

作成日 2020/05/24

## 01. 公式ドキュメントを読む

トップ => [https://matplotlib.org/](https://matplotlib.org/)

> Matplotlib is a Python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms.\
> Matplotlib can be used in Python scripts, the Python and IPython shells, the Jupyter notebook, web application servers, and four graphical user interface toolkits.

概要 => [https://matplotlib.org/contents.html](https://matplotlib.org/contents.html)

API => [https://matplotlib.org/api/index.html](https://matplotlib.org/api/index.html)

## 02. matplotlib.pyplot 解析

[https://matplotlib.org/api/pyplot_summary.html](https://matplotlib.org/api/pyplot_summary.html)

メソッド

- `axvline` ... Add a vertical line across the axes.
- `figure` ... Create a new figure.
- `subplot` ... Add a subplot to the current figure.
- `hist` ... Plot a histogram.
- `plot` ... Plot y versus x as lines and/or markers.
- `scatter` ... A scatter plot of y vs.
- `title` ... Set a title for the axes.
- `show` ... Display all figures.
- `xlim` ... Set the x limits of the current axes.

引数の解説

- `figure(figsize)` ... (横幅, 高さ）をインチで指定
- `plot(x,y)` ... plot x and y using default line style and color
- `plot(x,y,'bo')` ... plot x and y using blue circle markers
- `subplot(nrows,ncols,index)` ... 行数、列数、インデックス（※）
- `hist(x,bins,orientation)` ... 値、置き場、向き
- `scatter(x,y)` ... スカラーか配列

※ インデックスは、左上から右下へ Z の形に数えた自分の位置。最初は 1 番

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(10,5))

plt.subplot(1,2,1)
plt.hist(x = x, bins = 15)
plt.title("X")

plt.show()

plt.figure(figsize=(10,10))
plt.subplot(2,2,2)
plt.scatter(x = x, y = y)
plt.title("Joint Distribution of X and Y")

plt.show()
```

## 03. エラー対処

### ヒストグラムを描画しようとしたらエラー

```text
ValueError: max must be larger than min in range parameter.
```

欠損値を除けばエラーが出なくなる

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame(np.random.randn(4,4))
df.iloc[0,0] = np.nan

plt.hist(df[0])
# => エラー

plt.hist(df[0].dropna())
# => 問題なし
```

## 04. 解説記事を読む

[matplotlib 入門 \- りんごがでている](http://bicycle1885.hatenablog.com/entry/2014/02/14/023734)

### 写経してみる

Colabratory に書く

[Matplotlib の基本\.ipynb \- Colaboratory](https://colab.research.google.com/drive/1L_HSod0i1KVxzQaDFHqockVmGA37giYs)

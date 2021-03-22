# Numpy 勉強

作成日 2020/05/04

公式トップ => [NumPy — NumPy](https://numpy.org/)

ドキュメントトップ => [NumPy Documentation](https://numpy.org/doc/)

最新バージョン => 1.18.4

## 01. Random sampling を学ぶ

[Random sampling \(numpy\.random\) — NumPy v1\.18 Manual](https://numpy.org/doc/1.18/reference/random/index.html)

numpy.random の静的メソッドを呼ぶと、それは Generator クラスのインスタンスメソッドを呼ぶことになると解釈した

```python
import numpy as np

random_students = np.random.choice(population, sampSize)
```

### numpy.random.choice

[numpy\.random\.Generator\.choice — NumPy v1\.18 Manual](https://numpy.org/doc/1.18/reference/random/generated/numpy.random.Generator.choice.html)

`Generator.choice(a, size=None, replace=True, p=None, axis=0)`

パラメーター

- `a` ... ndarray => choose from, int => choose range
- `size` ... int => output shape

### numpy.random.normal

[numpy\.random\.normal — NumPy v1\.18 Manual](https://numpy.org/doc/stable/reference/random/generated/numpy.random.normal.html)

`numpy.random.normal(loc=0.0, scale=1.0, size=None)`

パラメーター

- `loc` ... float => Mean of the distribution
- `scale` ... float => standard deviation
- `size` ... int => output shape

### numpy.arange

[numpy\.arange — NumPy v1\.18 Manual](https://numpy.org/doc/stable/reference/generated/numpy.arange.html)

`numpy.arange(start, stop, step)`

パラメーター

- `start` ... number => start of interval
- `stop` ... number => end of interval
- `step` ... number => spacing between values

### numpy.around

[numpy\.around — NumPy v1\.18 Manual](https://numpy.org/doc/stable/reference/generated/numpy.around.html)

`numpy.around(a,decimals=0)`

パラメーター

- `a` ... array_like => input data
- `decimals` ... int => Number of decimal places

# Scipy.stats

作成日 2020/05/24

## Statistical functions (scipy.stats)

公式トップ => [https://docs.scipy.org/doc/scipy/reference/stats.html](https://docs.scipy.org/doc/scipy/reference/stats.html)

## scipy.stats.ttest_ind

[https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html)

`scipy.stats.ttest_ind(a, b, axis=0, equal_var=True, nan_policy='propagate')`

## scipy.stats.bernoulli

[https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.bernoulli.html](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.bernoulli.html)

```python
import scipy.stats as st

st.bernoulli.pmf(k, p)
```

pmf = Probability mass function

k は 0 か 1 で、k=0 の場合は 1-p を返す。k=1 の場合は、p を返す

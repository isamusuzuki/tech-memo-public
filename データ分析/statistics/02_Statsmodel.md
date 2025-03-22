# Statsmodel

作成日 2020/06/06

## Statsmodel とは

めちゃくちゃ大きなライブラリ

公式 => [https://www.statsmodels.org/stable/index.html](https://www.statsmodels.org/stable/index.html)

ドキュメント => [https://www.statsmodels.org/stable/api.html](https://www.statsmodels.org/stable/api.html)

```python
import numpy as np
import statsmodels.api as sm

# 平均が0、標準偏差が1の正規分布のサンプル1000個
normalRandomVariables = np.random.normal(0, 1, 1000)

x = sm.stats.DescrStatsW(normalRandomVariable)

(x.mean, x.std)
# => (0.028567098405328402, 1.041269921036999)
```

## Distributions

[https://www.statsmodels.org/stable/distributions.html](https://www.statsmodels.org/stable/distributions.html)

`ECDF(x, side)` ... Return the Emprical CDF of an array as step function\
かいつまんで結果を表す

## OLS (Ordinary Least Squares)

Ordinary Least Squares is a method for estimating the unknown parameters in a linear regression model.

[https://www.statsmodels.org/stable/generated/statsmodels.regression.linear_model.OLS.html](https://www.statsmodels.org/stable/generated/statsmodels.regression.linear_model.OLS.html)

`OLS.from_formula(formula, data, subset=None, drop_cols=None, *args, **kwargs)` ... Create a Model from a formula and dataframe.

```python
import numpy as np
import statsmodels.api as sm

da = pd.read_csv("nhanes_2015_2016.csv")

# Drop unused columns, drop rows with any missing values.
vars = ["BPXSY1", "RIDAGEYR", "RIAGENDR", "RIDRETH1", "DMDEDUC2", "BMXBMI",
        "SMQ020", "SDMVSTRA", "SDMVPSU"]
da = da[vars].dropna()

da["RIAGENDRx"] = da.RIAGENDR.replace({1: "Male", 2: "Female"})

model = sm.OLS.from_formula("BPXSY1 ~ RIDAGEYR + RIAGENDRx", data=da)
res = model.fit()
print(res.summary())
```

## GLM (Generalized Linear Models)

we will be using this suite of functions to carry out logistic regression.

Logistic regression is used when our target variable is a binary outcome, or a classification of two groups, which can be denoted as group 0 and group 1.

[https://www.statsmodels.org/stable/glm.html](https://www.statsmodels.org/stable/glm.html)

[https://www.statsmodels.org/stable/generated/statsmodels.genmod.generalized_linear_model.GLM.html](https://www.statsmodels.org/stable/generated/statsmodels.genmod.generalized_linear_model.GLM.html)

`GLM.from_formula(formula, data, subset=None, drop_cols=None, *args, **kwargs)` ... Create a Model from a formula and dataframe.

```python
import numpy as np
import statsmodels.api as sm

da = pd.read_csv("nhanes_2015_2016.csv")

da["smq"] = da.SMQ020.replace({2: 0, 7: np.nan, 9: np.nan})
model = sm.GLM.from_formula("smq ~ RIAGENDRx", family=sm.families.Binomial(), data=da)
res = model.fit()
print(res.summary())
```

## GEE (Generalized Estimating Equations)

Generalized Estimating Equations (GEE) fit marginal linear models, and estimate intraclass correlation.

[Generalized Estimating Equations — statsmodels](https://www.statsmodels.org/stable/gee.html)

[statsmodels\.genmod\.generalized_estimating_equations\.GEE — statsmodels](https://www.statsmodels.org/stable/generated/statsmodels.genmod.generalized_estimating_equations.GEE.html)

`GEE.from_formula(formula, groups, data, subset=None, time=None, offset=None, exposure=None, *args, **kwargs)` ... Create a Model from a formula and dataframe.

```python
import numpy as np
import statsmodels.api as sm

da = pd.read_csv("nhanes_2015_2016.csv")

da["group"] = 10*da.SDMVSTRA + da.SDMVPSU
model = sm.GEE.from_formula("BPXSY1 ~ 1", groups="group", cov_struct=sm.cov_struct.Exchangeable(), data=da)
res = model.fit()
print(res.cov_struct.summary())
```

## MIXEDLM (Multilevel Models)

Similarly to GEEs, we use multilevel models when there is potential for outcomes to be grouped together which is not uncommon when using various sampling methods to collect data.

```python
import numpy as np
import statsmodels.api as sm

da = pd.read_csv("nhanes_2015_2016.csv")

for v in ["BPXSY1", "RIDAGEYR", "BMXBMI", "smq", "SDMVSTRA"]:
    model = sm.GEE.from_formula(v + " ~ 1", groups="group",
           cov_struct=sm.cov_struct.Exchangeable(), data=da)
    result = model.fit()
    print(v, result.cov_struct.summary())
```

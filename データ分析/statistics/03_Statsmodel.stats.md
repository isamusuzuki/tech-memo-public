# Statsmodel.stats

作成日 2020/06/06

## stats とは

さまざまな統計検定とツールを集めたセクション

公式 => [https://www.statsmodels.org/stable/stats.html](https://www.statsmodels.org/stable/stats.html)

## DescrStatsW

[https://www.statsmodels.org/stable/generated/statsmodels.stats.weightstats.DescrStatsW.html](https://www.statsmodels.org/stable/generated/statsmodels.stats.weightstats.DescrStatsW.html)

`statsmodels.stats.weightstats.DescrStatsW(data, weights=None, ddof=0)` ... descriptive statistics and tests with weights for case weights

これを使ってインスタンス化したクラスには、\
`.mean`や`.std`, `.sum` といったプロパティがあるだけでなく\
`.zconfint_mean()` のようなメソッドもある

### DescriStatsW.zconfint_mean

[https://www.statsmodels.org/stable/generated/statsmodels.stats.weightstats.DescrStatsW.zconfint_mean.html](https://www.statsmodels.org/stable/generated/statsmodels.stats.weightstats.DescrStatsW.zconfint_mean.html)

`DescrStatsW.zconfint_mean(alpha=0.05, alternative='two-sided')` ... two-sided confidence interval for weighted mean of data

パラメーター

- alpha ... significance level, default 0.05
- alternative ... alternative hypothesis for the test

戻り値

- lower, upper ... lower and upper bound of confidence interval

```python
import pandas as pd
import statsmodels.api as sm

# Import data that will be used to construct confidence interval of population mean
df = pd.read_csv("https://raw.githubusercontent.com/UMstatspy/UMStatsPy/master/Course_1/Cartwheeldata.csv")

# Generate confidence interval for a population mean
sm.stats.DescrStatsW(df["CWDistance"]).zconfint_mean()
# => (76.57715593233024, 88.38284406766977)
```

## proportion_confit

[https://www.statsmodels.org/stable/generated/statsmodels.stats.proportion.proportion_confint.html](https://www.statsmodels.org/stable/generated/statsmodels.stats.proportion.proportion_confint.html)

`statsmodels.stats.proportion.proportion_confint(count, nobs, alpha=0.05, method='normal')` ... confidence interval for a binomial proportion

パラメーター

- count ... number of successes in nobs trials
- nobs ... the number of trials
- alpha ... significance level, default 0.05

戻り値

- ci_low, ci_upp ... lower and upper confidence level

```python
import statsmodels.api as sm

# Observer population proportion
p = .85

# Size of population
n = 659

# the lower and upper bounds of a 95% confidence interval of population proportion
sm.stats.proportion_confint(n * p, n)
# => (0.8227378265796143, 0.8772621734203857)
```

## propotions_ztest

[https://www.statsmodels.org/stable/generated/statsmodels.stats.proportion.proportions_ztest.html](https://www.statsmodels.org/stable/generated/statsmodels.stats.proportion.proportions_ztest.html)

`statsmodels.stats.propotion.proportions_ztest(count, nobs, value=None, alternative='two-sided', prop_var=False)` ... Test for propotions based on normal (z) test

パラメーター

- count ... number of successes in nobs trials
- nobs ... the number of trials
- value ... value of the null hypothesis equal to the propotion
- alternative ... ['tow-sided', 'smaller', 'larger']
- prop_var ... False or float in (0,1)

戻り値

- zstat ... test statistic for the z-test
- p-value ... p-value for the z-test

自分なりの解釈

- count: 調査で「はい」と答えた人の数
- nobs: 調査した人の数
- count/nobs: 実際の割合は勝手に計算されるから引数に入れない
- value: 自分が指標にしたい割合
- alternative: 出したい結論「同一でない」「以上」「以下」
- p-value: 帰無仮説が起こりうる確率

```python
import statsmodels.api as sm

# Population size
n = 1018

# Null hypothesis population proportion
pnull = .52

# Observe population proportion
phat = .56

# Calculate test statistic and p-value
sm.stats.proportions_ztest(phat * n, n, pnull)
# => (2.571067795759113, 0.010138547731721065)
```

## ttest_ind

[https://www.statsmodels.org/stable/generated/statsmodels.stats.weightstats.ttest_ind.html](https://www.statsmodels.org/stable/generated/statsmodels.stats.weightstats.ttest_ind.html)

`statsmodels.stats.weightstats.ttest_ind(x1, x2, alternative='two-sided', usevar='pooled', weights=(None, None), value=0)` ... ttest independent sample

戻り値

- zstat ... test statistic for the z-test
- p-value ... p-value for the z-test
- df ... degrees of freedom used in the t-test

## ztest

[https://www.statsmodels.org/stable/generated/statsmodels.stats.weightstats.ztest.html](https://www.statsmodels.org/stable/generated/statsmodels.stats.weightstats.ztest.html)

`statsmodels.stats.weightstats.ztest(x1, x2=None, value=0, alternative='two-sided', usevar='pooled', ddof=1.0)` ... test for mean based on normal distribution, one or two samples In the case of two samples, the samples are assumed to be independent.

パラメーター

- x1 ... first of the two independent samples
- x2 ... second of the two independent samples
- value ... one sample ならば、帰無仮説下の x1 の平均値。two sample ならば、帰無仮説下の差異

戻り値

- zstat ... test statistic for the z-test
- p-value ... p-value for the z-test

```python
import pandas as pd
import statsmodels.api as sm

# Import data that will be used to construct confidence interval of population mean
df = pd.read_csv("https://raw.githubusercontent.com/UMstatspy/UMStatsPy/master/Course_1/Cartwheeldata.csv")

# Using the dataframe imported above, perform a hypothesis test for population mean
sm.stats.ztest(df["CWDistance"], value = 80, alternative = "larger")
# => (0.8234523266982029, 0.20512540845395266)
```

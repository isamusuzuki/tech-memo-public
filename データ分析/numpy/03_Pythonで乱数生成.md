# Python で乱数を生成する

作成日 2020/05/24

公式ドキュメント => [https://docs.python.org/ja/3/library/random.html](https://docs.python.org/ja/3/library/random.html)

## random 疑似乱数を生成する

シードを設定してから、ランダムさせると同じ結果（シークエンス）になる

```python
import random
random.seed(1234)
random.random()
# => 0.9664535356921388（1回目は常にこの数）
```

### 実数分布

以下の関数は特定の実数値分布を生成する

- `random()` ... 0.0 以上、1.0 以下のランダムな浮動小数点数を返す
- `uniform(a, b)` ... a 以上、b 以下のランダムな浮動小数点数を返す
- `normalvariate(mu, sigma)` ... mu が平均で、sigma が標準偏差になる正規分布

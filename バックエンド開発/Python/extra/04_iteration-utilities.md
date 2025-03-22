# iteration-utilities を使いこなす

作成日 2022/01/12

## iteration-utilities とは

リストの中の重複を一発で取り出せたりする、ユーティリティ群

PyPIのサイト => [https://pypi.org/project/iteration-utilities/](https://pypi.org/project/iteration-utilities/)

```python
from iteration_utilities import duplicates, unique_everseen

a = 'AABBCCDA'
b = list(duplicates(a))
# => [A', 'B', 'C', 'A']

c = list(unique_everseen(duplicates(a)))
# => ['A', 'B', 'C']
```

# イテレータとジェネレータ

作成日 2019/9/20

## 01. イテレータ iterator

要素を反復して取り出すことができるインターフェイス\
組み込み関数 `iter()` => イテレーターオブジェクトを返す

```python
hoge = [1, 2, 3]
hoge_iter = iter(hoge)
hoge_iter.__next__() #=> 1
hoge_iter.__next__() #=> 2
hoge_iter.__next__() #=> 3
hoge_iter.__next__() #=> StopIteration例外発生
```

## 02. ジェネレータ generator

イテレータの一種で、1 要素を取り出そうとするたびに処理を行い、要素をジェネレートする\
ジェネレータ関数の中では`yield`を使う。`return`は使えない\
ジェネレータは一度 for ループで回すと 2 回目以降の for ループでは要素が出てこない

```python
def my_generator();
  yield 1
  yield 2
  yield 3

gen = my_generator()
gen.__next__() #=> 1
gen.__next__() #=> 2
gen.__next__() #=> 3
gen.__next__() #=> StopIteration例外発生
```

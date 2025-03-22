# MeCabで形態素解析を行う

作成日 2020/05/28

## MeCabをインストールする

[mecab\-python3 · PyPI](https://pypi.org/project/mecab-python3/)

```bash
cd ~/everflow
source bin/activate

pip install mecab-python3
```

Linuxの場合は、これだけでバイナリーもインストールされる

[SamuraiT/mecab\-python3: mecab\-python\. you can find original version here //taku910\.github\.io/mecab/](https://github.com/SamuraiT/mecab-python3)

> Binary wheels are available for MacOS X, Linux, and Windows (64bit) are installed by default when you use pip:

Windows も大丈夫そうではある

## MeCabを試す

```python
import MeCab

wakati = MeCab.Tagger('-Owakati')
wakame = wakati.parse('レディースファッション').split()
print(wakame) # => ['レディース', 'ファッション']
```

やりたいことができそうな気になってきた

## 参考記事を読む

[PythonとMeCabで形態素解析\(on Windows\) \- Qiita](https://qiita.com/menon/items/f041b7c46543f38f78f7)

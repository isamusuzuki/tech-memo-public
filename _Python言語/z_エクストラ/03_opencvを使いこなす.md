# opencv を使いこなす

作成日 2019/11/20

## 01. opencv とは

Open Source Computer Vision Library\
インテルが開発・公開したオープンソースのコンピュータビジョン向けライブラリ

公式ドキュメント => [https://opencv-python-tutroals.readthedocs.io/en/latest/](https://opencv-python-tutroals.readthedocs.io/en/latest/)

日本語ドキュメント => [http://labs.eecs.tottori-u.ac.jp/sd/Member/oyamada/OpenCV/html/index.html](http://labs.eecs.tottori-u.ac.jp/sd/Member/oyamada/OpenCV/html/index.html)

### opencv をインストールする

```bash
pip install opencv-python-headless
```

現時点では、python3.8 に対応していない。python3.7 の環境でインストールする必要がある

## 02. opencv で画像を読み込む

画像を読み込んだ後は`numpy.ndarray`が手に入る

ndarry のリファレンス => [numpy\.ndarray — NumPy v1\.17 Manual](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.html)

```python
import cv2

# カラー画像として読み込む
im2 = cv2.imread('images/daruma.png', cv2.IMREAD_COLOR)
print(f'im2.ndim => {im2.ndim}')  # => 3
print(f'im2.shape => {im2.shape}')  # => (32, 32, 3)

# グレー画像として読み込む
im1 = cv2.imread('images/daruma.png', cv2.IMREAD_GRAYSCALE)
print(f'im1.ndim => {im1.ndim}')  # => 2
print(f'im1.shape => {im1.shape}')  # => (32, 32)

# 透過情報込みで読み込む
im3 = cv2.imread('images/daruma.png', cv2.IMREAD_UNCHANGED)
print(f'im3.ndim => {im3.ndim}')  # => 3
print(f'im3.shape => {im3.shape}')  # => (32, 32, 4)
```

### 透過の扱い

透過のある PNG 画像を、グレースケールで読むと、何もないところ（透過なところ）は`0`（黒）になる。\
カラーとして読んでも、`[0 0 0]`（黒）になっている。\
追加チャンネル含みで読むと、始めてアルファチャンネルが得られる。\
アルファチャンネルの`0`は透明、`255`は完全な不透明である

## 03. 白い背景を切り落とすためにトライしたコード

```python
import cv2

# カラー画像をグレースケール画像として読み込む
# numpyのndarrayが手に入る
im = cv2.imread('images/yaki_curry.jpg', cv2.IMREAD_GRAYSCALE)
print(f'im.ndim 何次元 => {im.ndim}')
print(f'im.shape (y, x) => {im.shape}')

# 0～255までのグレースケールの中で、240～255は白とみなす
# True, Falseに置換されたndarrayが手に入る
WHITES = im > 240

# 上から下に、白い線が垂れているとみなす。その長さのリストをつくる
from_top = []
for x in range(0, im.shape[1]):
    w = WHITES[:, x]
    from_top.append(list(w).index(False) if False in w else len(w))
print(f'from_top => {min(from_top)} <= {len(from_top)}')

# 下から上に、白い線が垂れているとみなす。その長さのリストをつくる
from_bottom = []
for x in range(0, im.shape[1]):
    w = WHITES[::-1, x]
    from_bottom.append(list(w).index(False) if False in w else len(w))
print(
    f'from_bottom => {min(from_bottom)} <= {len(from_bottom)}')

# 左から右に、白い線が垂れているとみなす。その長さのリストをつくる
from_left = []
for y in range(0, im.shape[0]):
    w = WHITES[y, :]
    from_left.append(list(w).index(False) if False in w else len(w))
print(
    f'from_left => {min(from_left)} <= {len(from_left)}')

# 右から左に、白い線が垂れているとみなす。その長さのリストをつくる
from_right = []
for y in range(0, im.shape[0]):
    w = WHITES[y, ::-1]
    from_right.append(list(w).index(False) if False in w else len(w))
print(
    f'from_right => {min(from_right)} <= {len(from_right)}')
```

### ndarray をスライスする

- `WHITES[:, 8]`は、左から 8 本目の、上から下に垂れている白い線
- `WHITES[::-1, 8]`は、左から 8 本目の、下から上に上がっている白い線
- `WHITES[8, :]`は、上から 8 本目の、左から右に引かれた白い線
- `WHITES[8, ::-1]`は、上から 8 本目の、右から左に引かれた白い線

### 最初に False が登場したときのインデックスを知る

- `list.index('a')`は、リストの中で最初に登場した a のインデックスを返す
- ndarray に`index()`メソッドはないが、`list(ndarray)`でリストに変換できる
- False がないときは、最後まで白い線が引かれたとする

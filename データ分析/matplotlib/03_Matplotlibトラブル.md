# Matplotlib トラブルシューティング

作成日 2020/06/07

## plt.show()でエラーが出た

```python
import matplotlib.pyplot as plt

import numpy as np

x = np.linspace(-4, 4, 100)
plt.plot(x, np.sin(x))
plt.show()
# => UserWarning: Matplotlib is currently using agg, which is a non-GUI backend, so cannot show the figure.
```

### とりあえずの解決法

画像ファイルを保管する

```python
import matplotlib.pyplot as plt

import numpy as np

x = np.linspace(-4, 4, 100)
plt.plot(x, np.sin(x))
plt.savefig('temp/apple.png')
```

### 抜本的な解決法

tkをインストールするといいらしい

```bash
sudo apt install python3-tk -y
```

# logging を使う

作成日 2021/06/28

## 01. logging の掟

- `logging.debug()` のように、`logging`モジュール付属の関数を使ってログを吐いてはいけない
- `logger = getLogger(__name__)` のように、自分のロガーを用意してからログを吐く
- しかし、自分のロガーを用意しただけではどこにも出力されない。ハンドラーが必要
- ロガーが入力であり、ハンドラーが出力だから、両方で`setLevel()` する必要がある

```python
from logging import DEBUG, getLogger

logger = getLogger(__name__)
logger.setLevel(DEBUG)

logger.debug("debug")
logger.info("info")
logger.warn("warn")
logger.error("error")
logger.critical("critical")
```

## 02. コンソールにログを出力する

```python
from logging import DEBUG, StreamHandler, Formatter, getLogger

logger = getLogger(__name__)
logger.setLEVEL(DEBUG)

sh = StreamHandler()
sh.setLevel(DEBUG)
fmt = Formatter(
    '%(asctime)0.19s - %(module)s [%(levelname)s] %(message)s'
)
sh.setFormatter(fmt)
logger.addHandler(sh)

logger.debug('Hello, World!')
```

## 03. ログファイルにログを出力する

```python
from datetime import datetime
from logging import DEBUG, FileHandler, Formatter, getLogger
import pytz

logger = getLogger(__name__)
logger.setLevel(DEBUG)

now = datetime.now(pytz.timezone('Asia/Tokyo'))
daily_str = now.strftime("%Y%m%d")
fh = FileHandler(f'temp/{now_str}.log')
fh.setLevel(DEBUG)
fmt = Formatter(
    '%(asctime)0.19s - %(module)s [%(levelname)s] %(message)s'
)
fh.setFormatter(fmt)
logger.addHandler(fh)

logger.debug('Hello, World!')
```

## 04. `logging.getLogger()` を自分なりに拡張する

- 一番外側にいるモジュールがロガーを用意する
- 一番外側にいるモジュールがロガーの設定をいじる
- 使い回しされるモジュールは引数でロガーをもらう

```text
--avocado/
    |--apps/
    |   |--gyakusu.py
    |   `--logger_a.py
    `--main.py
```

logger_a.py

```python
from datetime import datetime
from logging import \
    DEBUG, FileHandler, Formatter, Logger, StreamHandler, getLogger

import pytz


def getLoggerA(
        name: str, level: int = DEBUG,
        type: str = 'console', prefix: str = 'main',
        cycle: str = 'monthly') -> Logger:
    logger = getLogger(name)
    logger.setLevel(level)
    if not logger.hasHandlers():
        # Formatterは共通
        fmt = Formatter(
            '%(asctime)0.19s - %(module)s [%(levelname)s] %(message)s'
        )
        # FileHandlerを準備して組み込む
        if type == 'both' or type == 'file':
            now = datetime.now(pytz.timezone('Asia/Tokyo'))
            daily_str = now.strftime('%Y%m%d')
            monthly_str = now.strftime('%Y%m')
            if cycle == 'daily':
                log_file = f'temp/{prefix}_{daily_str}.log'
            else:
                log_file = f'temp/{prefix}_{monthly_str}.log'
            fh = FileHandler(log_file)
            fh.setLevel(level)
            fh.setFormatter(fmt)
            logger.addHandler(fh)

        # StreamHandlerの準備して組み込む
        if type == 'both' or type == 'console':
            sh = StreamHandler()
            sh.setFormatter(fmt)
            logger.addHandler(sh)
    return logger
```

main.py

```python
from apps.gyakusu import gyakusu
from apps.logger_a import getLoggerA

from fire import Fire


class Run():
    def __init__(self) -> None:
        self.logger = getLoggerA(
            __name__, type='both',
            prefix='test', cycle='daily')

    def job1(self):
        _ = gyakusu(1, self.logger)
        _ = gyakusu(2, self.logger)
        _ = gyakusu(3, self.logger)
        # _ = gyakusu(0, self.logger)
        self.logger.info('===DONE===')


if __name__ == '__main__':
    Fire(Run)
```

gyakusu.py

```python
from logging import Logger


def gyakusu(number: int, logger: Logger) -> None:
    try:
        g_num = str(1/number)
        logger.debug(f'{number} => {g_num}')
        return g_num
    except Exception as err:
        logger.exception(f'{err}')
        raise Exception(f'{err}')
```

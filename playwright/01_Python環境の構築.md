# Python 環境の構築

作成日 2021/10/01

## 01. Playwright 1.15 をインストールする

OS は Ubuntu または WSL Ubuntu を想定

参照した公式サイト => [Getting Started \| Playwright](https://playwright.dev/python/docs/intro)

```bash
cd ~/PROJECT-NAME
source venv/bin/activate

pip install playwright
```

## 02. Playwright 用のブラウザのインストール

```bash
playwright --version
# => Version 1.15.0-1631797286000

# Chromium/Webkit/Firefox の3つのブラウザをインストールするので時間がかかる
# python -m playwright install

# Chromiumだけをインストールする
playwright install chromium
# => Downloading Playwright build of chromium v920619
```

## 03. 動作確認

```bash
cd ~/playwright-dojo
source venv/bin/activate

python firstscript.py
# => temp/firstscript.png が生成されれば成功
```

firstscript.py

```python
from dotenv import load_dotenv

from fire import Fire

from playwright.sync_api import sync_playwright


def run():
    with sync_playwright() as p:
        browser = p.chromium.launch(
            # headless=False, # => ブラウザを表示する
            # slow_mo=50      # => ブラウザをゆっくり動かす
        )
        page = browser.new_page()
        page.goto('http://whatsmyuseragent.org/')
        page.screenshot(path='temp/firstscript.png')
        browser.close()


if __name__ == '__main__':
    load_dotenv()
    Fire(run)
```

## クラウドサーバーで Playwright が動くようにする

GCP の VM インスタンスである Ubuntu サーバーで走らせてみる

```bash
cat /etc/os-release
# => NAME="Ubuntu"
# => VERSION="20.04.3 LTS (Focal Fossa)"

cd ~/PROJECT-NAME
source venv/bin/activate

python fisrtscript.py
# => Host system is missing a few dependencies to run browsers.
# => Please install them with the following command:
# =>
# => sudo npx playwright install-deps
# =>
# => <3 Playwright Team

# Node.js 環境はインストールしていないので、
# 推測で Python 版を動かす
playwright install-deps

python fisrtscript.py
# done => temp/firstscript.png
```

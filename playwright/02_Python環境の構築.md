# Python 環境の構築

作成日 2021/02/28、更新日 2021/03/09

## 01. Playwright をインストールする

参照した公式サイト => [Getting Started \| Playwright](https://playwright.dev/python/docs/intro)

```bash
# Linuxの場合
cd ~/playwright-dojo
python3 -m venv venv
source venv/bin/activate
pip install wheel
pip install -r requirements.txt

# Windowsの場合
cd ~/playwright-dojo
python -m venv venv
source venv/Scripts/activate
pip install -r requirements.txt
```

### requirements.txt の設定例

```text
autopep8
fire
flake8
flake8-import-order
openpyxl
pandas
playwright
PyMySQL
python-dotenv
pytz
xlrd
```

### .vscode/settings.json の設定例

```json
{
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.flake8Enabled": true,
  "python.linting.flake8Path": "venv/bin/flake8",
  "python.linting.lintOnSave": true,
  "python.formatting.provider": "autopep8",
  "python.formatting.autopep8Path": "venv/bin/autopep8",
  "editor.formatOnSave": true,
  // Linuxの場合
  "terminal.integrated.env.linux": {
    "PYTHONPATH": "/home/{{YOURNAME}}/playwright-dojo"
  },
  // Windowsの場合
  "terminal.integrated.env.windows": {
    "PYTHONPATH": "C:\\Users\\{{YOURNAME}}\\playwright-dojo"
  }
}
```

### Playwright 用のブラウザのインストール

```bash
# Chromium/Webkit/Firefox の3つのブラウザをインストールするので時間がかかる
# python -m playwright install

# Chromiumだけをインストールする
playwright install chromium
```

### インストールされたブラウザのありか

- Windows の場合 ... `~\AppData\Local\ms-playwright`
- Linux の場合 ... `~/.cache/ms-playwright`

インストール後のファイル構造を確認する

```text
--.cache/
    `--ms-playwright/
        |--chromium-854489/
        |   |--INSTALLATION_COMPELTE
        |   `--chrome-linux/
        |       `--chrome
        `--ffmpeg-1005/
```

### インストールされた Playwright のバージョンを確認する

```bash
cd ~/playwright-dojo
source venv/bin/activate

playwright --version
# => Version 1.9.1-1614225150000
```

### Python コードの動作確認

```bash
cd ~/playwright-dojo
source venv/bin/activate

python firstscript.py
# => temp/firstscript.py.png が生成されれば成功
```

firstscript.py

```python
from dotenv import load_dotenv

from fire import Fire

from playwright.sync_api import sync_playwright


def run():
    with sync_playwright() as p:
        browser = p.chromium.launch(
            headless=False, slow_mo=50
        )
        page = browser.new_page()
        page.goto('http://whatsmyuseragent.org/')
        page.screenshot(path='temp/firstscript.py.png')
        browser.close()


if __name__ == '__main__':
    load_dotenv()
    Fire(run)
```

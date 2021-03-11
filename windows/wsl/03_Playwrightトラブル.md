# Playwright トラブルシューティング

作成日 2020/10/05

## 1. `Permission Error`が発生

`venv/lib/python3.8/site-packages/playwright/drivers/driver-linux` を起動できるようにファイル属性を変更する必要があった

```bash
cd ~/PROJECT-NAME
cd venv/lib/python3.8/site-packages/playwright/drivers
chmod 775 driver-linux
```

## 2. `Playwright.helper.Error: Host system is missing dependencies!`が発生

足りないモジュールをインストールする必要があった

```bash
sudo apt install gstreamer1.0-libav
```

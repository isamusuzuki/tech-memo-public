---
tags: mattermost
---

# Mattermost内向きのウェブフック

作成日 2019/11/13

ドキュメント => [https://docs.mattermost.com/developer/webhooks-incoming.html](https://docs.mattermost.com/developer/webhooks-incoming.html)


## 01. 内向きのウェブフックを作成する手順


Web ページ ＞ メインメニュー（左上の横三本） ＞ 統合機能をクリック

![20190607_mattermost1.png](https://imgur.com/LpOtfCp.png)

統合機能のページ ＞ 内向きのウェブフックをクリック

![20190607_mattermost2.png](https://imgur.com/pOYBFFC.png)

新しく作成するときに必要な項目は 4 つ

![20190607_mattermost3.png](https://imgur.com/fwdWZlh.png)

URL が誕生

![20190607_mattermost4.png](https://imgur.com/9naYWG7.png)

## 02. Pythonサンプルコード

`apps/mattermost.py`

```python=
import json
import urllib.error
import urllib.request


def hook_to_mm():
    hook_url = (
        'https://mattermost.eg-wms.com/hooks/'
        'abcdedfghijklmnopqrstu'
    )
    data = {
        'username': 'Amachan1号',
        'icon_url': 'https://imgur.com/VLSfc8p.png',
        'text': (
            '@i_suzuki\n'
            '##### 売れ筋ランキング 深堀作業が完了しました\n'
        )
    }
    headers = {
        'Content-Type': 'application/json'
    }
    req = urllib.request.Request(
        hook_url, json.dumps(data).encode('utf-8'), headers)
    with urllib.request.urlopen(req) as res:
        _ = res.read()


if __name__ == "__main__":
    hook_to_mm()
```



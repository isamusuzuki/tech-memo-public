# Fabric

作成日 2021/03/02

## 01. 技術記事の中に Fabric という単語を発見する

[仮想サーバ 17 万台、物理サーバ 9 万台　「ヤフオク\!」「Yahoo\! JAPAN」を支えるヤフーの IT インフラ運用術 \(1/2\) \- ITmedia NEWS](https://www.itmedia.co.jp/news/articles/2008/13/news042.html)

> ヤフーは構築だけではなく、テストの自動化にも取り組んでいるが、そのプロセスにも改善の余地がある。例えば、同社では現在、タスク実行ツール「Fabric」を用いてテストコードを各サーバに送り込み、100 項目以上のテストを自動で実行している。

## 02. Fabric とは

公式サイト => [Welcome to Fabric\! — Fabric documentation](http://www.fabfile.org/)

> Fabric is a high level Python (2.7, 3.4+) library designed to execute shell commands remotely over SSH, yielding useful Python objects in return:

```python
from fabric import Connection

result = Connection('web1.example.com').run('uname -s', hide=True)
msg = "Ran {0.command!r} on {0.connection.host}, got stdout:\n{0.stdout}"
print(msg.format(result))
# => Ran 'uname -s' on web1.example.com, got stdout:
# => Linux
```

> It builds on top of Invoke (subprocess command execution and command-line features) and Paramiko (SSH protocol implementation), extending their APIs to complement one another and provide additional functionality.

### Getting Started を読む

[Getting started — Fabric documentation](https://docs.fabfile.org/en/2.5/getting-started.html)

### 日本語の解説記事を読む

[fabric2 のインストール手順と簡単な使い方 \- Qiita](https://qiita.com/Esfahan/items/1e4bdf14b4a22263a1cf)

> fabric1 と 2 は別物だと考えた方がよいです。fabric2 は、Python の invoke ライブラリのラッパーなので、invoke のドキュメントも参考になります。

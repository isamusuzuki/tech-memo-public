# Flask サーバーを構築する

作成日 2019/12/10

## 01. Python と Flask をセットアップする

```bash
# Pythonをインストールする
choco install python

# バージョンを確認する
python --version
#=> Python 3.7.3
pip --version
#=> pip 19.0.3 from c:\python37\lib\site-packages\pip (python 3.7)

# pipパッケージを更新する
python -m pip install --upgrade pip
pip --version
#=> pip 19.1.1 from c:\python37\lib\site-packages\pip (python 3.7)

# プロジェクトのフォルダを作成する
cd ~
mkdir andy
cd andy

# モジュールをインストールする
echo flask > requirements.txt
pip install -r requirements.txt
pip freeze > constraints.txt
```

## 02. Flask アプリをテストランする

`hello.py`ファイル

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello World!'
```

テストランしてみる

```bash
# Git Bashの場合
set FLASK_APP=hello.py flask run

# PowerShellの場合
set-item env:FLASK_APP -value hello.py
flask run
```

テストランしてわかったこと

-   ローカルからならば接続できる（`http://127.0.0.1:5000`）
-   別マシンからは接続できない（`http://192.168.2.81:5000`）
-   利用目的が社内 Web サーバーならば、WSGI サーバーは必須

## 03. waitress をセットアップする

### waitress を選択した理由

-   uWSGI は、コンパイルが必要
-   gunicorn は、Windows に非対応
-   waitress は、ピュア Python だから、Windows でも動く

### waitress をインストールする

公式サイト => [https://docs.pylonsproject.org/projects/waitress/en/stable/](https://docs.pylonsproject.org/projects/waitress/en/stable/)

紹介記事 => [Waitress is a Alternative of Flask and Gunicorn for windows](https://www.docketrun.com/blog/waitress-alternative-flask-gunicorn/)

Waitress をインストールする => `pip install waitress`

### waitress を試す

`server.py`ファイル

```python
from flask import Flask
from waitress import serve
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello World!'

if __name__ == '__main__':
    serve(app, host='0.0.0.0', port=80)
```

waitress を起動する

```bash
python server.py
# => Serving on http://ANDY:80
```

別マシンからは接続できた（`http://192.168.2.81`）！

## 04. Python スクリプトを常駐させる

### nssm とは

exe でも、batch でも、なんでもサービス化できる便利ツール

公式サイト => [NSSM \- the Non\-Sucking Service Manager](https://nssm.cc/usage)

インストール => `choco install nssm`

### nssm でサービスを作成する

-   `nssm install andy` => GUI が登場する
-   Application タブ
    -   Path ... `C:\Python37\python.exe`
    -   Startup directory ... `C:\Users\user\andy`
    -   Arguments ... `server.py`
-   Log on タブ
    -   Log on as > This Account ... `user`
    -   Password, Confirm ... いつものやつ
-   I/O タブ
    -   Output ... `C:\Users\user\Desktop\andy.log`
    -   Error ... `C:\Users\user\Desktop\andy.log`
-   Install service ボタン

コントロールパネル ＞ 管理ツール ＞ サービス ＞ andy がいた\
右クリックして開始したら、デスクトップにログファイルが登場した

一度作成したサービスは変更できない。\
削除するしかない。削除した後は再起動が必要

## 05. ファイアウォールルールを変更する

コントロールパネル ＞ 表示方法：小さいアイコン ＞ Windows Defender ファイアーウォール\
左枠 ＞ 詳細設定 ＞　「セキュリティが強化された Windows Defender ファイアウォール」が登場\
左枠 ＞ 受信の規則 ＞ 右枠 ＞ 操作 ＞ 新しい規則... ＞ 「新規の受信の規則ウィザード」が登場

| ステップ               | 設定                                                 |
| ---------------------- | ---------------------------------------------------- |
| 規則の種類             | ポート                                               |
| プロトコルおよびポート | TCP 80                                               |
| 操作                   | 接続を許可する                                       |
| プロファイル           | ドメイン、プライベート、パブリック全てでチェックオン |
| 名前                   | HTTP                                                 |

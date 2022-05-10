# vscode で Python スクリプトをデバッグする

作成日 2020/03/25、更新日 2020/07/25

## 01. 公式チュートリアルを写経する

[Get Started Tutorial for Python in Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial)

> In this tutorial, you use Python 3 to create the simplest Python "Hello World" application in Visual Studio Code.
>
> This tutorial introduces you to VS Code as a Python environment, primarily how to edit, run, and debug code through the following tasks:
>
> - Write, run, and debug a Python "Hello World" Application
> - Learn how to install packages by creating Python virtual environments
> - Write a simple Python script to plot figures within VS Code

### 以下の項目は読み飛ばす

- Prerequisites
- Install Visual Studio Code and the Python Extension
- Install a Python interpreter
- Verify the Python installation
- Start VS Code in a project (workspace) folder
- Select a Python interpreter
- Create a Python Hello World source code file
- Run Hello World

### Configure and run the debugger

hello.py

```python
msg = 'Hello World'
print(msg)
```

最初に、`hello.py`の 2 行目にブレークポイントをセットする。\
その行にカーソルを持っていって、`F9`を押してもいいし、\
エディターの左ガーターをクリックしてもいい。\
ブレークポイントをセットすると、ガーターに赤い丸が登場する

次に`F5`を押して、デバッガーを初期化する。\
このファイルを初めてデバッグするときは、\
コマンドパレットに設定メニューが登場する\
設定メニューは、`Python File`を選ぶ

デバッガーは最初のブレークポイントで止まる\
左枠の変数枠を見ると、Locals に msg 変数があるのがわかる\
デバッグツールバーが登場する。アイコンの役割は次の通り

| 順番 | 意味      | キーボードショートカット |
| :--: | --------- | ------------------------ |
|  1   | continue  | F5                       |
|  2   | step over | F10                      |
|  3   | step into | F11                      |
|  4   | step out  | Shift + F11              |
|  5   | restart   | Ctrl + Shift + F5        |
|  6   | stop      | Shift + F5               |

.vscode ファイルに launch.json が出来上がった\
`launch.json`は、共通の設定メニューとなる

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Current File",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal"
    }
  ]
}
```

### 似たような操作の別バージョン

左端サイドメニュー 上から 4 番目 RUN をクリックする\
変数窓の上に、ドロップダウンリストがあり、その中に "Add Configuration" がある\
これをクリックすると、`.vscode/launch.json`ファイルが作られる\
ドロップダウンリストの一番上にある、"Python: Current File"の左にある\
緑の三角形をクリックすれば、デバッグが始まる

### わからないこと

どうして自分がやった時は、デバッガーが止まらないのか？\
止まらないし、変数窓に何もでてこない

新しく作成したフォルダに、`hello.py`だけを置いて、デバッグしてみたら\
ちゃんと止まったし、変数窓にも msg が登場した

この違いは何なのか？

## 02. 公式ドキュメントを読む

[Debugging configurations for Python apps in Visual Studio Code](https://code.visualstudio.com/docs/python/debugging)

## 03. 関連記事を読む

[VSCode で Python をデバッグする \- Qiita](https://qiita.com/bigengelt/items/780440a146e6a3bdffd4)

[Visual Studio Code のデバッグで、virtualenv, venv の Python 仮想環境を使う \- Qiita](https://qiita.com/watahani/items/7c1b3b6c470b2f08bf51)

## 04. 仮想環境下にある.py ファイルが止まらない問題

[VSCode で Python 仮想環境のデバッグ実行時にブレークポイントで止まらない問題 \- Qiita](https://qiita.com/EngineerShiba/items/6c7d2b97829a4c4092d7)

settings.json

```json
{
  "python.pythonPath": "実行環境により変化"
}
```

launch.json

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Current File",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal",
      "pythonPath": "${config:python.pythonPath}",
      "stopOnEntry": true
    }
  ]
}
```

[Debugger Not Stopping at Breakpoints in VS Code for Python \- Stack Overflow](https://stackoverflow.com/questions/56794940/debugger-not-stopping-at-breakpoints-in-vs-code-for-python)

> Setting "justMyCode": false makes it stop at breakpoint:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Debug Current File",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal",
      "stopOnEntry": true,
      "justMyCode": false
    }
  ]
}
```

### justMyCode キーに関する説明

公式ドキュメントの中にあった

[Debugging configurations for Python apps in Visual Studio Code](https://code.visualstudio.com/docs/python/debugging)

> justMyCode\
> When omitted or set to true (the default), restricts debugging to user-written
> code only. Set to false to also enable debugging of standard library functions.

つまりは、自分が書いたコードなのに、ライブラリだと思われているからスルーされるということか？

合点が行く説明ではある

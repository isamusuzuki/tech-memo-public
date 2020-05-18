# vscode の Python 環境構築

作成日 2019/11/21

## 01. venv で仮想環境を構築する

```bash
cd ~/YOUR-PROJECT

# venvの仮想環境を構築
/c/Python37/python -m venv .

# 仮想環境の有効化
source Scripts/activate

# バージョン番号の確認
python --version
# => Python 3.7.4

# モジュールのインストール
pip install -r requirements.txt
pip freeze > constraints.txt

#　2回目以降のインストール
pip install -r requirements.txt -c constraints.txt

# 仮想環境の無効化
deactivate
```

## 02. autopep8 と flake8 を利用して、Python コードを自動整形する

- autopep8 ... PEP8 スタイルガイドに準拠するように自動的にフォーマットする
- flake8 ... 文法をチェックし、自動的にコードレビューする
- flake8-import-order ... import の順番をチェックする

`requirements.txt`ファイル

```text
autopep8
flake8
flake8-import-order
```

`.vscode/settings.json`ファイル

```json
{
  "python.pythonPath": "${workspaceFolder}\\Scripts\\python.exe",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.flake8Enabled": true,
  "python.linting.flake8Path": "${workspaceFolder}\\Scripts\\flake8.exe",
  "python.linting.lintOnSave": true,
  "python.formatting.provider": "autopep8",
  "python.formatting.autopep8Path": "${workspaceFolder}\\Scripts\\autopep8.exe",
  "editor.formatOnSave": true
}
```

`.gitignore`ファイル

```text
__pycache__/
.vscode/
Include/
Lib/
Scripts/
pyvenv.cfg
```

### Python コードを自動整形＆コードレビューさせる

1. ファイルを保存するたびに、自動で整形とレビューが行われる
1. `Ctrl + Shift + S` を叩く
1. `Ctrl + Shift + P`でコマンドパレットを開いてから、"Format Document"を実行する

### 【オマケ】いったんパッケージを全部アンインストールする

現在インストールされているパッケージの一覧を書き出し、\
それを使って、まとめてアンインストールする。\
`-y`をつけることで、毎回聞かれる削除確認を省く

```bash
pip freeze > temp.txt
pip uninstall -r temp.txt -y
```

## 03. ターミナルを表示した時に、自動で仮想環境を有効にする

- `Ctrl + Shift + p`で、コマンドパレットを表示する
- `Python: Select Interpreter`を選ぶ
- 候補の中から`.\Scripts\python.exe`が選ばれていることを確認する
- 左下に`Python 3.7.4 64-bit`が表示されていることを確認する
- この状態でターミナルを起動すると、`source Scripts/activate`が実行されてから登場する
